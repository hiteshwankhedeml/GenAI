# LangGraph Introduction - Code 2

* We keep adding messages to state
* And this state we flow to different functions
* If we need answer of one node in another node, then we can use this
* To get only the last output we will use invoke, we can use stream if we want output of each node
* To debug we can use stream

```python
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_community.document_loaders import TextLoader, DirectoryLoader
from langchain_community.vectorstores import Chroma
from langchain.text_splitter import RecursiveCharacterTextSplitter

loader=DirectoryLoader("../data",glob="./*.txt",loader_cls=TextLoader)
docs=loader.load()
text_splitter=RecursiveCharacterTextSplitter(
    chunk_size=500,
    chunk_overlap=200
)
new_docs = text_splitter.split_documents(documents=docs)
doc_strings = [doc.page_content for doc in new_docs]
# List of contents of all the pages

db = Chroma.from_documents(new_docs, embeddings)
retriever = db.as_retriever(search_kwargs={"k": 3})

db = Chroma.from_documents(new_docs, embeddings)

retriever = db.as_retriever(search_kwargs={"k": 3})

query = "why scientist was working hard for what kind of vaccines?"
docs = retriever.get_relevant_documents(query)
print(docs[0].metadata)
print(docs[0].page_content)

AgentState={}
AgentState["messages"]=[]
AgentState
# {'messages': []}

AgentState["messages"].append("hi")
AgentState["messages"].append("how are you?")
AgentState["messages"].append("what are you doing?")
AgentState
# {'messages': ['hi', 'how are you?', 'what are you doing?']}

def function_1(AgentState):
    message=AgentState["messages"]
    
    question=message[-1]
    
    complete_prompt="Your task is to provide only the brief answer based on the user query. \
        Don't include too much reasoning. Following is the user query: " + question
    
    response = llm.invoke(complete_prompt)
    
    AgentState['messages'].append(response.content) # appending LLM call response to the AgentState
    
    #print(AgentState)
    
    return AgentState
   
def function_2(AgentState):
    messages = AgentState['messages']
    
    question = messages[0] ## Fetching the user question
    
    template = """Answer the question based only on the following context:
    {context}

    Question: {question}
    """
    prompt = ChatPromptTemplate.from_template(template)

    retrieval_chain = (
        {"context": retriever, "question": RunnablePassthrough()}
        | prompt
        | llm
        | StrOutputParser()
        )
    result = retrieval_chain.invoke(question)
    return result
    
workflow=Graph()
workflow.add_node("LLM", function_1)
workflow.add_node("RAGtool", function_2)
workflow.add_edge('LLM', 'RAGtool')
workflow.set_entry_point("LLM")
workflow.set_finish_point("RAGtool")
app2 = workflow.compile()

display(Image(app2.get_graph().draw_mermaid_png())) 

app2.invoke({"messages":["what is a meaning of 'cancer from prolonged exposure to burn pits ravaged Heath’s lungs and body'?"]})

for output in app2.stream({"messages":["what is a meaning of 'cancer from prolonged exposure to burn pits ravaged Heath’s lungs and body'?"]}):
    for key,value in output.items():
        print(f"here is output from {key}")
        print("_______")
        print(value)
        print("\n")
#here is output from LLM
# _______
# {'messages': ["what is a meaning of 'cancer from prolonged exposure to
# burn pits ravaged Heath’s lungs and body'?", "Heath's lungs and body were 
# severely damaged by cancer caused by prolonged exposure to burn pits."]}
#
#
#here is output from RAGtool
#_______
#Heath's lungs and body were severely damaged by cancer that
# developed due to long-term exposure to burn pits.

```

*

    <figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
