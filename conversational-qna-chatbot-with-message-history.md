---
hidden: true
---

# 🔴 Conversational QnA Chatbot with message history

* In many Q\&A applications we want to allow the user to have a back-and-forth conversation, meaning the application needs some sort of "memory" of past questions and answers, and some logic for incorporating those into its current thinking.
* 2 approaches:
  * Chains, in which we always execute a retrieval step;
  * Agents, in which we give an LLM discretion over whether and how to execute a retrieval step (or multiple steps).

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Create a RAG**</mark>
* <mark style="color:purple;background-color:purple;">**contextualize\_q\_system\_prompt ⇒  Create a contextualized system prompt, its job is rephrasing user questions if it depends on chat history, it does not answer the question**</mark>
* <mark style="color:purple;background-color:purple;">**contextualize\_q\_prompt ⇒  Prompt template with above system prompt, message history and human input**</mark>
* ```
  contextualize_q_prompt = ChatPromptTemplate.from_messages(
      [
          ("system", contextualize_q_system_prompt),
          MessagesPlaceholder("chat_history"),
          ("human", "{input}"),
      ]
  )
  ```
* <mark style="color:purple;background-color:purple;">**history\_aware\_retriever = history-aware retriever which reformulates follow-up questions into standalone ones before retrieving documents**</mark>
* <mark style="color:purple;background-color:purple;">**qa\_prompt ⇒ Prompt template with system prompt, message holder and human input**</mark>
* <mark style="color:purple;background-color:purple;">**question\_answer\_chain ⇒ Document chain using llm and qa\_prompt**</mark>
* <mark style="color:purple;background-color:purple;">**rag\_chain ⇒ retriever using history\_aware\_retriever and question\_answer\_chain**</mark>
* <mark style="color:purple;background-color:purple;">**For History we will be using session id and Runnable like before only**</mark>

```python
import os
from dotenv import load_dotenv
load_dotenv()
from langchain_groq import ChatGroq

groq_api_key=os.getenv("GROQ_API_KEY")

llm=ChatGroq(groq_api_key=groq_api_key,model_name="Llama3-8b-8192")

llm

os.environ['HF_TOKEN']=os.getenv("HF_TOKEN")
from langchain_huggingface import HuggingFaceEmbeddings
embeddings=HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")

from langchain_chroma import Chroma
from langchain_community.document_loaders import WebBaseLoader
from langchain_core.prompts import ChatPromptTemplate
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain

# 1. Load, chunk and index the contents of the blog to create a retriever.
import bs4
loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs=dict(
        parse_only=bs4.SoupStrainer(
            class_=("post-content", "post-title", "post-header")
        )
    ),
)

docs=loader.load()
docs

text_splitter=RecursiveCharacterTextSplitter(chunk_size=1000,chunk_overlap=200)
splits=text_splitter.split_documents(docs)
vectorstore=Chroma.from_documents(documents=splits,embedding=embeddings)
retriever=vectorstore.as_retriever()
retriever

## Prompt Template
system_prompt = (
    "You are an assistant for question-answering tasks. "
    "Use the following pieces of retrieved context to answer "
    "the question. If you don't know the answer, say that you "
    "don't know. Use three sentences maximum and keep the "
    "answer concise."
    "\n\n"
    "{context}"
)

prompt = ChatPromptTemplate.from_messages(
    [
        ("system", system_prompt),
        ("human", "{input}"),
    ]
)

question_answer_chain=create_stuff_documents_chain(llm,prompt)
rag_chain=create_retrieval_chain(retriever,question_answer_chain)

response=rag_chain.invoke({"input":"What is Self-Reflection"})
response

from langchain.chains import create_history_aware_retriever
from langchain_core.prompts import MessagesPlaceholder

contextualize_q_system_prompt = (
    "Given a chat history and the latest user question "
    "which might reference context in the chat history, "
    "formulate a standalone question which can be understood "
    "without the chat history. Do NOT answer the question, "
    "just reformulate it if needed and otherwise return it as is."
)
contextualize_q_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", contextualize_q_system_prompt),
        MessagesPlaceholder("chat_history"),
        ("human", "{input}"),
    ]
)

history_aware_retriever=create_history_aware_retriever(llm,retriever,contextualize_q_prompt)
history_aware_retriever

qa_prompt = ChatPromptTemplate.from_messages(
    [
        ("system", system_prompt),
        MessagesPlaceholder("chat_history"),
        ("human", "{input}"),
    ]
)


question_answer_chain=create_stuff_documents_chain(llm,qa_prompt)
rag_chain=create_retrieval_chain(history_aware_retriever,question_answer_chain)

from langchain_core.messages import AIMessage,HumanMessage
chat_history=[]
question="What is Self-Reflection"
response1=rag_chain.invoke({"input":question,"chat_history":chat_history})

chat_history.extend(
    [
        HumanMessage(content=question),
        AIMessage(content=response1["answer"])
    ]
)

question2="Tell me more about it?"
response2=rag_chain.invoke({"input":question,"chat_history":chat_history})
print(response2['answer'])

from langchain_community.chat_message_histories import ChatMessageHistory
from langchain_core.chat_history import BaseChatMessageHistory
from langchain_core.runnables.history import RunnableWithMessageHistory

store = {}


def get_session_history(session_id: str) -> BaseChatMessageHistory:
    if session_id not in store:
        store[session_id] = ChatMessageHistory()
    return store[session_id]


conversational_rag_chain = RunnableWithMessageHistory(
    rag_chain,
    get_session_history,
    input_messages_key="input",
    history_messages_key="chat_history",
    output_messages_key="answer",
)

conversational_rag_chain.invoke(
    {"input": "What is Task Decomposition?"},
    config={
        "configurable": {"session_id": "abc123"}
    },  # constructs a key "abc123" in `store`.
)["answer"]

conversational_rag_chain.invoke(
    {"input": "What are common ways of doing it?"},
    config={"configurable": {"session_id": "abc123"}},
)["answer"]


```
