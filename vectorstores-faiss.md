# 🟢 VectorStores - FAISS

* <mark style="color:purple;background-color:purple;">**Facebook AI Similarity Search**</mark> (Faiss) is a library for efficient similarity search and clustering of dense vectors.
* It contains algorithms that search in sets of vectors of any size, up to ones that possibly do not fit in RAM. It also contains supporting code for evaluation and parameter tuning.
* <mark style="color:purple;background-color:purple;">**We can also store and load the vectors**</mark>
* <mark style="color:purple;background-color:purple;">**We can do similairty search using db as well as retriever**</mark>

<mark style="color:purple;background-color:purple;">**As a Retriever:**</mark>

* <mark style="color:purple;background-color:purple;">**We can also convert the vectorstore into a Retriever class. ⇒ db.as\_retriever()**</mark>
* <mark style="color:purple;background-color:purple;">**This allows us to easily use it in other LangChain methods, which largely work with retrievers**</mark>
* <mark style="color:purple;background-color:purple;">**VectorDB stores your embedded documents and does similarity search.**</mark>
* <mark style="color:purple;background-color:purple;">**Retriever wraps the VectorDB to provide a clean, flexible interface for fetching relevant docs.**</mark>
* <mark style="color:purple;background-color:purple;">**Your RAG pipeline or LLM queries the retriever, not the VectorDB directly.**</mark>

**Similarity Search with score:**

* There are some FAISS specific methods. One of them is similarity\_search\_with\_score, which allows you to return not only the documents but also the distance score of the query to them.
* The returned distance score is L2 distance.
* Therefore, a lower score is better.

```python
from langchain_community.document_loaders import TextLoader
from langchain_community.vectorstores import FAISS
from langchain_community.embeddings import OllamaEmbeddings
from langchain_text_splitters import CharacterTextSplitter

loader=TextLoader("speech.txt")
documents=loader.load()
text_splitter=CharacterTextSplitter(chunk_size=1000,chunk_overlap=30)
docs=text_splitter.split_documents(documents)

embeddings=OllamaEmbeddings()
db=FAISS.from_documents(docs,embeddings)
db

### querying 
query="How does the speaker describe the desired outcome of the war?"
docs=db.similarity_search(query)
docs[0].page_content

# As a retriever
retriever=db.as_retriever()
docs=retriever.invoke(query)
docs[0].page_content

docs_and_score=db.similarity_search_with_score(query)
docs_and_score

# We can also pass vectors instead of text
embedding_vector=embeddings.embed_query(query)
embedding_vector

docs_score=db.similarity_search_by_vector(embedding_vector)
docs_score

### Saving And Loading
db.save_local("faiss_index")

new_db=FAISS.load_local("faiss_index",embeddings,allow_dangerous_deserialization=True)
docs=new_db.similarity_search(query)
```
