# RAG - Code

* RunnablePassThrough is for taking dynamic question from the user&#x20;

```python
import langchain
from langchain import hub
from langchain_community.document_loaders import WebBaseLoader
from langchain_community.vectorstores import FAISS
from langchain_groq import ChatGroq
from langchain_community.embeddings import HuggingFaceBgeEmbeddings
from langchain_core.output_parsers import StrOutputParser
from langchain.text_splitter import RecursiveCharacterTextSplitter
from dotenv import load_dotenv,find_dotenv
import os
import bs4
import pprint

load_dotenv(find_dotenv())

os.environ["GROQ_API_KEY"] =  os.getenv("GROQ_API_KEY")

loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs=dict(
        #filter specific parts of the webpage, improving efficiency.
        parse_only=bs4.SoupStrainer(
            class_=("post-content", "post-title", "post-header")
        )
    ),
)

docs=loader.load()
docs[0].metadata # docs[0].metadata
print(docs[0].page_content) # Web page contents

llm=ChatGroq(model="llama3-8b-8192")

model_name="BAAI/bge-small-en"
model_kwargs={"device": "cpu"}
encode_kwargs={"normalize_embeddings": True}
hf_embeddings=HuggingFaceBgeEmbeddings(
    model_name=model_name, model_kwargs=model_kwargs, encode_kwargs=encode_kwargs
)

text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
splits = text_splitter.split_documents(docs)
len(splits) # 66

vectorstore = FAISS.from_documents(documents=splits,embedding=hf_embeddings)
retriever=vectorstore.as_retriever()

prompt = hub.pull("rlm/rag-prompt")

prompt.messages
# [HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=['context',
# 'question'], input_types={}, partial_variables={},
# template="You are an assistant for question-answering tasks.
# Use the following pieces of retrieved context to answer the question.
# If you don't know the answer, just say that you don't know.
# Use three sentences maximum and keep the answer concise.
# \nQuestion: {question} \nContext: {context} \nAnswer:"),
# additional_kwargs={})]

pprint.pprint(prompt.messages)

from langchain_core.runnables import RunnablePassthrough

def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)
    
# Chain
rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

rag_chain.invoke("What is Task Decomposition?")

```
