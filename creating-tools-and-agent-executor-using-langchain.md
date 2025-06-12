# Creating tools and Agent Executor using LangChain

* Tool will be doing some functionality
* When we want to use we need to use API wrapper
* In hub we can get different prompts
* [https://smith.langchain.com/hub/](https://smith.langchain.com/hub/)

```python
## Arxiv--Research
## Tools creation
from langchain_community.tools import ArxivQueryRun,WikipediaQueryRun
from langchain_community.utilities import WikipediaAPIWrapper,ArxivAPIWrapper

## Used the inbuilt tool of wikipedia
api_wrapper_wiki=WikipediaAPIWrapper(top_k_results=1,doc_content_chars_max=250)
wiki=WikipediaQueryRun(api_wrapper=api_wrapper_wiki)
wiki.name

api_wrapper_arxiv=ArxivAPIWrapper(top_k_results=1,doc_content_chars_max=250)
arxiv=ArxivQueryRun(api_wrapper=api_wrapper_arxiv)
print(arxiv.name)

tools=[wiki,arxiv]

## Custom tools[RAG Tool]
from langchain_community.document_loaders import WebBaseLoader
from langchain_community.vectorstores import FAISS
from langchain_openai import OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

loader=WebBaseLoader("https://docs.smith.langchain.com/")
docs=loader.load()
documents=RecursiveCharacterTextSplitter(chunk_size=1000,chunk_overlap=200).split_documents(docs)
vectordb=FAISS.from_documents(documents,OpenAIEmbeddings())
retriever=vectordb.as_retriever()
retriever

from langchain.tools.retriever import create_retriever_tool
retriever_tool=create_retriever_tool(retriever,"langsmith-search","Search any information about Langsmith ")

retriever_tool.name

tools=[wiki,arxiv,retriever_tool]

## Run all this tools with Agents and LLM Models

## Tools, LLM-->AgentExecutor
from langchain_groq import ChatGroq
from dotenv import load_dotenv
import openai
load_dotenv()
import os

groq_api_key=os.getenv("GROQ_API_KEY")
openai.api_key=os.getenv("OPENAI_API_KEY")

llm=ChatGroq(groq_api_key=groq_api_key,model_name="Llama3-8b-8192")

## Prompt Template
from langchain import hub
prompt=hub.pull("hwchase17/openai-functions-agent")
prompt.messages
# [SystemMessagePromptTemplate(prompt=PromptTemplate(input_variables=[], 
# input_types={}, partial_variables={}, template='You are a helpful assistant'), 
# additional_kwargs={}),
# MessagesPlaceholder(variable_name='chat_history', optional=True),
# HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=['input'], 
# input_types={}, partial_variables={}, template='{input}'), additional_kwargs={}),
# MessagesPlaceholder(variable_name='agent_scratchpad')]

## Agents
from langchain.agents import create_openai_tools_agent
agent=create_openai_tools_agent(llm,tools,prompt)
agent

## Agent Executer
from langchain.agents import AgentExecutor
agent_executor=AgentExecutor(agent=agent,tools=tools,verbose=True)
agent_executor

agent_executor.invoke({"input":"Tell me about Langsmith"})
# This will invoke lang-smith retriever

agent_executor.invoke({"input":"What is machine learning"})

agent_executor.invoke({"input":"What's the paper 1706.03762 about?"})
```
