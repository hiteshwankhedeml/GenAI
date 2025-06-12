# 🟢 Pinecone - Code

* <mark style="color:purple;background-color:purple;">**API Key required, can be generated on pinecone site**</mark>
* <mark style="color:purple;background-color:purple;">**In free plan, you can create only 1 project**</mark>
* <mark style="color:purple;background-color:purple;">**Can be configured for any cloud**</mark>
* Use langchain to add data
* Specify filter for pre-filering
* uuids are optionable

```python
!pip install pinecone-client


from langchain.document_loaders import PyPDFDirectoryLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from pinecone import Pinecone, ServerlessSpec
from langchain.vectorstores import Pinecone

loader=PyPDFDirectoryLoader(r"C:\\E_COURSE\\genai_bootcamp\\data")

data=loader.load()

text_splitter=RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)

text_chunks=text_splitter.split_documents(data)

from langchain_google_genai import GoogleGenerativeAIEmbeddings

google_embedding=GoogleGenerativeAIEmbeddings(model="models/embedding-001")

len(google_embedding.embed_query(text_chunks[0].page_content))

#-------Config PineCone Client
pc = Pinecone(api_key="pcsk_25ke2c_4fx6TiT7DKGx98jn")

index_name = "quickstart3"

pc.create_index(
    name=index_name,
    dimension=768, # Replace with your model dimensions
    metric="cosine", # Replace with your model metric
    spec=ServerlessSpec(
        cloud="aws",
        region="us-east-1"
    ) 
)


pc.list_indexes() # to get the list of indexes

# Create new index if it doesnt exists
# If it already exists then connect with it
import time

index_name = "langchain-test-index2"  # change if desired

existing_indexes = [index_info["name"] for index_info in pc.list_indexes()]

if index_name not in existing_indexes:
    pc.create_index(
        name=index_name,
        dimension=768,
        metric="cosine",
        spec=ServerlessSpec(cloud="aws", region="us-east-1"),
    )
    while not pc.describe_index(index_name).status["ready"]:
        time.sleep(1)

index = pc.Index(index_name)

from langchain_pinecone import PineconeVectorStore

vector_store = PineconeVectorStore(index=index, embedding=google_embedding)vector_store
 
from uuid import uuid4
 
uuids = [str(uuid4()) for _ in range(len(text_chunks))]
 
vector_store.add_documents(documents=text_chunks, ids=uuids)
 
 
results = vector_store.similarity_search(
    "what is llama2 and how it is different from mistral?",
    k=2,
    filter={"source": "C:\\data\\llama2.pdf"},
)
 
for res in results:
    print(f"* {res.page_content} [{res.metadata}]")
 
```
