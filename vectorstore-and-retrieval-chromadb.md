# VectorStore and Retrieval - ChromaDB

* Chroma is a AI-native open-source vector database focused on developer productivity and happiness. Chroma is licensed under Apache 2.0.
* <mark style="color:purple;background-color:purple;">**When we save it to the disk, it uses sqlite3**</mark>
* [https://python.langchain.com/v0.2/docs/integrations/vectorstores/](https://python.langchain.com/v0.2/docs/integrations/vectorstores/)

```python
## building a sample vectordb
from langchain_chroma import Chroma
from langchain_community.document_loaders import TextLoader
from langchain_community.embeddings import OllamaEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

loader = TextLoader("speech.txt")
data = loader.load()
data

# Split
text_splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=0)
splits = text_splitter.split_documents(data)

embedding=OllamaEmbeddings()
vectordb=Chroma.from_documents(documents=splits,embedding=embedding)
vectordb

## query it
query = "What does the speaker believe is the main reason the United States should enter the war?"
docs = vectordb.similarity_search(query)
docs[0].page_content

## Saving to the disk
vectordb=Chroma.from_documents(documents=splits,embedding=embedding,persist_directory="./chroma_db")

# load from disk
db2 = Chroma(persist_directory="./chroma_db", embedding_function=embedding)
docs=db2.similarity_search(query)
print(docs[0].page_content)

## similarity Search With Score
docs = vectordb.similarity_search_with_score(query)
docs

### Retriever option
retriever=vectordb.as_retriever()
retriever.invoke(query)[0].page_content
```
