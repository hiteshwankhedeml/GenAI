# LangGraph Code - 3

* [https://github.com/sunnysavita10/genai\_bootcamp/blob/main/langgraph-agents/langraph\_introduction.ipynb](https://github.com/sunnysavita10/genai_bootcamp/blob/main/langgraph-agents/langraph_introduction.ipynb)
* new\_docs will have chunked data
* doc\_string will  be the entire text data
*

````python
loader=DirectoryLoader("../data",glob="./*.txt",loader_cls=TextLoader)
docs=loader.load()
text_splitter=RecursiveCharacterTextSplitter(
    chunk_size=100,
    chunk_overlap=50
)
new_docs = text_splitter.split_documents(documents=docs)
doc_strings = [doc.page_content for doc in new_docs]

db = Chroma.from_documents(new_docs, embeddings)
retriever = db.as_retriever(search_kwargs={"k": 3})

query = "Tell me about India's Industrial Growth?"
docs = retriever.get_relevant_documents(query)
print(docs[0].metadata)
print(docs[0].page_content)

from typing import TypedDict, Annotated,Sequence
import operator
from langchain_core.messages import BaseMessage
from langchain.prompts import PromptTemplate

class AgentState(TypedDict):
    messages:Annotated[Sequence[BaseMessage],operator.add]

from langchain_core.messages import HumanMessage

state=AgentState(messages=[HumanMessage(content="hi there")])
state
# {'messages': [HumanMessage(content='hi there', additional_kwargs={}, response_metadata={})]}

state=AgentState(messages=["hi"])
state
# {'messages': ['hi']}

from pydantic import BaseModel, Field

class TopicSelectionParser(BaseModel):
    Topic:str=Field(description="selected topic")
    Reasoning:str=Field(description="reasoning behind the topic")

pydantic_obj=TopicSelectionParser(Topic="india", Reasoning="india is growing country?")

pydantic_obj.Topic
# 'india'

pydantic_obj.Reasoning
# 'india is growing country?'

from langchain.output_parsers import PydanticOutputParser

parser=PydanticOutputParser(pydantic_object=TopicSelectionParser)
print(parser.get_format_instructions())
# The output should be formatted as a JSON instance that conforms to the JSON schema below.
# As an example, for the schema {"properties": {"foo": {"title": "Foo", "description": "a list of strings", "type": "array", "items": {"type": "string"}}}, "required": ["foo"]}
# the object {"foo": ["bar", "baz"]} is a well-formatted instance of the schema. The object {"properties": {"foo": ["bar", "baz"]}} is not well-formatted.
# Here is the output schema:
# ```
# {"properties": {"Topic": {"description": "selected topic", "title": "Topic", "type": "string"}, "Reasoning": {"description": "reasoning behind the topic", "title": "Reasoning", "type": "string"}}, "required": ["Topic", "Reasoning"]}

def function_1(state:AgentState):
    
    message=state["messages"]
    
    question=message[-1] 
    
    print("***********here is my question********")
    print(question)
    
    template="""
    Your task is to classify the given user query into one of the following categories: [Japan, Not Related]. 
    Only respond with the category name and nothing else.

    User query: {question}
    
    {format_instructions}
    """
    
    prompt = PromptTemplate(template=template,
                            input_variables=[question],
                            partial_variables={"format_instructions" : parser.get_format_instructions()}
                            )
    
    chain =prompt | llm | parser
    
    response = chain.invoke({"question":question,"format_instructions" : parser.get_format_instructions() })
    
    print("***********my response********")
    print(response)
    
    return {"messages":[response.Topic]}

state={"messages":["tell me about the japan's industrial growth?"]}

function_1(state)
# ***********my question********
# tell me about the japan's industrial growth?
# ***********my response********
# Topic='Japan' Reasoning="The query explicitly asks about Japan's industrial growth."
# {'messages': ['Japan']}

def router(state:AgentState):
    print("***********entering into router********")
    print("*********my state from router********")
    print(state)
    
    message=state["messages"]
    
    last_message=message[-1]
    
    print("***********last message********")    
    print(last_message)
    
    if "Japan" in last_message:
        return "RAG Call"
    else:
        return "Simple LLM Call"

def function_2(state:AgentState):
    print("*********my state from function_2(rag)********")
    print(state)
    
    
    print('-> Calling RAG ->')
    
    messages = state['messages']
    
    question = messages[0] ## Fetching the user question
    
    print(question)

    template = """Answer the question based only on the following context:
    {context}

    Question: {question}
    """
    prompt = ChatPromptTemplate.from_template(template)
    
    print(prompt)

    retrieval_chain = (
        {"context": retriever, "question": RunnablePassthrough()}
        | prompt
        | llm
        | StrOutputParser()
        )
    result = retrieval_chain.invoke(question)
    response={"messages": [result]}
    print("*********my state from function_2(rag)********")
    print(state)
    return response

def function_3(state:AgentState):
    print('-> Calling LLM ->')

    messages = state['messages']
    
    question = messages[0] ## Fetching the user question

    # Normal LLM call
    complete_query = "Anwer the follow question with your knowledge of the real world. Following is the user question: " + question
    response = llm.invoke(complete_query)
    response={"messages": [response.content]}
    print("*********my state from function_3(LLM)********")
    print(state)
    return response

from langgraph.graph import StateGraph, END

workflow=StateGraph(AgentState)
workflow.add_node("supervisor",function_1)
workflow.add_node("RAG",function_2)
workflow.add_node("LLM",function_3)
workflow.set_entry_point("supervisor")

workflow.add_conditional_edges(
    "supervisor",
    router,
    {
        "RAG Call": "RAG",
        "Simple LLM Call": "LLM"
    },
    
)

workflow.add_edge("RAG",END)
workflow.add_edge("LLM",END)
app=workflow.compile()

display(Image(app.get_graph().draw_mermaid_png()))

response=app.invoke({"messages":["can you tell me about the japan industrial growth with GDP?"]})
#***********my question********
#can you tell me about the japan industrial growth with GDP?
#***********my response********
#Topic='Japan' Reasoning="The query explicitly asks about Japan's industrial growth and GDP."
#***********entering into router********
#*********my state from router********
#{'messages': ['can you tell me about the japan industrial growth with GDP?', 'Japan']}
#***********last message********
#Japan
#*********my state from function_2(rag)********
#{'messages': ['can you tell me about the japan industrial growth with GDP?', 'Japan']}
#-> Calling RAG ->
#can you tell me about the japan industrial growth with GDP?
#input_variables=['context', 'question'] input_types={} partial_variables={} messages=[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=['context', 'question'], input_types={}, partial_variables={}, template='Answer the question based only on the following context:\n    {context}\n\n    Question: {question}\n    '), additional_kwargs={})]
#*********my state from function_2(rag)********
#{'messages': ['can you tell me about the japan industrial growth with GDP?', 'Japan']}

response
# {'messages': ['can you tell me about the japan industrial growth with GDP?',
# 'Japan',
# 'Japan experienced three quarters of contraction starting from April 2004, followed by a recovery.  There is also mention of "industrial revival hope".  However, no specific GDP figures or their relationship to industrial growth are provided.']}

response["messages"][-1]
# "Japan experienced three quarters of contraction starting from April 2004, 
# followed by a recovery. There are hopes for an industrial revival.  The provided 
# context doesn't give specific GDP numbers or link the recovery directly to industrial 
# growth."

response=app.invoke({"messages":["hi how are you?"]})
# ***********my question********
# hi how are you?
# ***********my response********
# Topic='Not Related' Reasoning='The query is a general greeting and does not mention Japan or any related topics.'
# ***********my router********
# ***********last message********
# Not Related
# -> Calling LLM ->

for output in app.stream({"messages":["what is a age of donald trump?"]}):
    for key,value in output.items():
        print(f"here is output from {key}")
        print("_______")
        print(value)
        print("\n")
# ***********my question********
# what is a age of donald trump?
# ***********my response********
# Topic='Not Related' Reasoning="The query asks about Donald Trump's age, which has no relation to Japan."
# ***********my router********
# ***********last message********
# Not Related
# here is output from supervisor
# _______
# {'messages': ['Not Related']}
# -> Calling LLM ->
# here is output from LLM
# _______
# {'messages': ['Donald Trump was born on June 14, 1946.  Therefore, as of October 26, 2023, he is 77 years old.']}
````

*

    <figure><img src=".gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

