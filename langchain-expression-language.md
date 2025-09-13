# Langchain Expression Language

* Composes chains of components
* It defines an allowed set of input types and corresponding output types
* We have standard runnable methods ⇒ invoke, stream , batch
* Using batch we can pass list of different inputs
* stream is used as llm inputs takes time, so using stream users will get some output
* chain = prompt | llm | outputparser
* Fallbacks can be attached to individual components as well as entire chain
* Can be run in parallel&#x20;
* Using langsmith it can be logged

Code:

* RunnableMap will take in a question and turn into a dictionary of question and context
* We can also bind parameters to runnables
* We bind function to llm
* For testing fallback, we are using old model here
* In fallback we will be passing list of other runnables, first core runnables tries to run (in this case simple\_chain) if it fails then it goes through the list in order till it gets success

```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI
from langchain.schema.output_parser import StrOutputParser

### Simple Chain
prompt = ChatPromptTemplate.from_template(
    "tell me a short joke about {topic}"
)
model = ChatOpenAI()
output_parser = StrOutputParser()

chain = prompt | model | output_parser
chain.invoke({"topic": "bears"})

### More complex chain
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import DocArrayInMemorySearch

vectorstore = DocArrayInMemorySearch.from_texts(
    ["harrison worked at kensho", "bears like to eat honey"],
    embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()

retriever.get_relevant_documents("where did harrison work?")

template = """Answer the question based only on the following context:
{context}

Question: {question}
"""
prompt = ChatPromptTemplate.from_template(template)

from langchain.schema.runnable import RunnableMap
chain = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(x["question"]),
    "question": lambda x: x["question"]
}) | prompt | model | output_parser

chain.invoke({"question": "where did harrison work?"})

inputs = RunnableMap({
    "context": lambda x: retriever.get_relevant_documents(x["question"]),
    "question": lambda x: x["question"]
})

inputs.invoke({"question": "where did harrison work?"})

### Bind
functions = [
    {
      "name": "weather_search",
      "description": "Search for weather given an airport code",
      "parameters": {
        "type": "object",
        "properties": {
          "airport_code": {
            "type": "string",
            "description": "The airport code to get the weather for"
          },
        },
        "required": ["airport_code"]
      }
    },
        {
      "name": "sports_search",
      "description": "Search for news of recent sport events",
      "parameters": {
        "type": "object",
        "properties": {
          "team_name": {
            "type": "string",
            "description": "The sports team to search for"
          },
        },
        "required": ["team_name"]
      }
    }
  ]

prompt = ChatPromptTemplate.from_messages(
    [
        ("human", "{input}")
    ]
)
model = ChatOpenAI(temperature=0).bind(functions=functions)
runnable = prompt | model
runnable.invoke({"input": "what is the weather in sf"})
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'weather_search', 'arguments': '{"airport_code":"SFO"}'}})

### Fallback
from langchain.llms import OpenAI
import json

simple_model = OpenAI(
    temperature=0, 
    max_tokens=1000, 
    model="gpt-3.5-turbo-instruct"
)
simple_chain = simple_model | json.loads

challenge = "write three poems in a json blob, ...."
simple_chain.invoke(challenge) # This gives error

model = ChatOpenAI(temperature=0)
chain = model | StrOutputParser() | json.loads

final_chain = simple_chain.with_fallbacks([chain])

```

