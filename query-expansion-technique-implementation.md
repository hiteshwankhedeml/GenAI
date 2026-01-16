# 🟢 Query Expansion Technique Implementation

Steps:

* Create query expansion chain&#x20;
  * query\_expansion\_chain=query\_expansion\_prompt| llm | StrOutputParser()
* Create a document chain
* We will be creating a rag pipeline which will have a runnable map and document chain
* Runnable map will have input and context
* context will be retriever.invoke(query\_expansion\_chain.invoke({"query": x\["input"]})
*

```python
from langchain.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS
from langchain.chat_models import init_chat_model
from langchain.prompts import PromptTemplate
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain.chains.retrieval import create_retrieval_chain
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnableMap

## step1 : Load and split the dataset
loader = TextLoader("langchain_crewai_dataset.txt")
raw_docs = loader.load()
splitter = RecursiveCharacterTextSplitter(chunk_size=300, chunk_overlap=50)
chunks = splitter.split_documents(raw_docs)

### step 2: Vector Store
embedding_model=HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
vectorstore=FAISS.from_documents(chunks,embedding_model)

## step 3:MMR Retriever
retriever=vectorstore.as_retriever(search_type="mmr",search_kwargs={"k":5})
retriever

## step 4 : LLM and Prompt

import os
from dotenv import load_dotenv
load_dotenv()

os.environ["OPENAI_API_KEY"]=os.getenv("OPENAI_API_KEY")

llm=init_chat_model("openai:o4-mini")
llm

# Query expansion
query_expansion_prompt = PromptTemplate.from_template("""
You are a helpful assistant. Expand the following query to improve document retrieval by adding relevant synonyms, technical terms, and useful context.

Original query: "{query}"

Expanded query:
""")

query_expansion_chain=query_expansion_prompt| llm | StrOutputParser()
query_expansion_chain

query_expansion_chain.invoke({"query":"Langchain memory"})
# 'Expanded query:\n\n("LangChain memory" OR "LangChain memory management" 
# OR "LangChain memory module" OR "persistent conversation memory" OR 
# "embeddings-based memory" OR "stateful chatbots" OR "session context persistence" 
# OR "RAG memory store" OR "LLM context window" OR "prompt memory" OR "memory retriever")  \nAND  \n("VectorStoreMemory" OR "ConversationBufferMemory" OR "ConversationSummaryMemory" OR "RedisMemory" OR "SQLMemory" OR "MemoryChain" OR "MemoryRouter")  \nAND  \n(FAISS OR Chroma OR Pinecone OR Weaviate OR Milvus OR "vector embedding store" OR "document chunking" OR "knowledge retrieval")'

# RAG answering prompt
answer_prompt = PromptTemplate.from_template("""
Answer the question based on the context below.

Context:
{context}

Question: {input}
""")

document_chain=create_stuff_documents_chain(llm=llm,prompt=answer_prompt)

# Step 5: Full RAG pipeline with query expansion
rag_pipeline = (
    RunnableMap({
        "input": lambda x: x["input"],
        "context": lambda x: retriever.invoke(query_expansion_chain.invoke({"query": x["input"]}))
    })
    | document_chain
)

# Step 6: Run query
query = {"input": "What types of memory does LangChain support?"}
print(query_expansion_chain.invoke({"query":query}))
response = rag_pipeline.invoke(query)
print("✅ Answer:\n", response)

# Step 6: Run query
query = {"input": "CrewAI agents?"}
print(query_expansion_chain.invoke({"query":query}))
response = rag_pipeline.invoke(query)
print("✅ Answer:\n", response)


```
