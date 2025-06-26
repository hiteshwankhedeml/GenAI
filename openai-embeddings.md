# 🟢 OpenAI Embeddings

* <mark style="color:purple;background-color:purple;">**This is paid API**</mark>
* <mark style="color:purple;background-color:purple;">**OpenAI ⇒ Playground ⇒ Embedding ⇒ to see available embeddings**</mark>
* <mark style="color:purple;background-color:purple;">**We use embeddings.embed\_query ⇒ to create embeddings**</mark>
* <mark style="color:purple;background-color:purple;">**Chroma.from\_documents ⇒ to create from document chunks, in this we need to pass embedding as well as chunks**</mark>

```python
import os
from dotenv import load_dotenv
load_dotenv()  #load all the environment variables

os.environ["OPENAI_API_KEY"]=os.getenv("OPENAI_API_KEY")

from langchain_openai import OpenAIEmbeddings
embeddings=OpenAIEmbeddings(model="text-embedding-3-large")
embeddings

text="This is a tutorial on OPENAI embedding"
query_result=embeddings.embed_query(text)
query_result
len(query_result) # 3072

embeddings_1024=OpenAIEmbeddings(model="text-embedding-3-large",dimensions=1024) 
# This will generate embedding of 1024

from langchain_community.document_loaders import TextLoader

loader=TextLoader('speech.txt')
docs=loader.load()

from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter=RecursiveCharacterTextSplitter(chunk_size=500,chunk_overlap=50)
final_documents=text_splitter.split_documents(docs)

## Vector Embedding And Vector StoreDB
from langchain_community.vectorstores import Chroma

db=Chroma.from_documents(final_documents,embeddings_1024)
db

### Retrieve the results from query vectorstore db
query="It will be all the easier for us to conduct ourselves as belligerents"
retrieved_results=db.similarity_search(query)
print(retrieved_results)
```
