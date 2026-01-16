---
hidden: true
---

# ✈️ End to End Project using Pinecone and LangChain

* Signup on pinecone
* Initial 2 to 3 instances are free
* Get API key
* Create index
* We have used 384 here as we will be using huggingface embeddings which by default gives embedding of 384 dimension
* BM25 uses tfidf techniques by default

```python
from langchain_community.retrievers import PineconeHybridSearchRetriever

import os
from pinecone import Pinecone,ServerlessSpec
index_name="hybrid-search-langchain-pinecone"
## initialize the Pinecone client
pc=Pinecone(api_key=api_key)

#create the index
if index_name not in pc.list_indexes().names():
    pc.create_index(
        name=index_name,
        dimension=384,  # dimensionality of dense model
        metric="dotproduct",  # sparse values supported only for dotproduct
        spec=ServerlessSpec(cloud="aws", region="us-east-1"),
    )

index=pc.Index(index_name)
index

## vector embedding and sparse matrix
import os
from dotenv import load_dotenv
load_dotenv()

os.environ["HF_TOKEN"]=os.getenv("HF_TOKEN")

from langchain_huggingface import HuggingFaceEmbeddings
embeddings=HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
embeddings

from pinecone_text.sparse import BM25Encoder

bm25_encoder=BM25Encoder().default()
bm25_encoder

sentences=[
    "In 2023, I visited Paris",
        "In 2022, I visited New York",
        "In 2021, I visited New Orleans",

]

## tfidf values on these sentence
bm25_encoder.fit(sentences)

## store the values to a json file
bm25_encoder.dump("bm25_values.json")

# load to your BM25Encoder object
bm25_encoder = BM25Encoder().load("bm25_values.json")

retriever=PineconeHybridSearchRetriever(embeddings=embeddings,sparse_encoder=bm25_encoder,index=index)

retriever.add_texts(
    [
    "In 2023, I visited Paris",
        "In 2022, I visited New York",
        "In 2021, I visited New Orleans",

]
)

retriever.invoke("What city did i visit first")
#[Document(page_content='In 2021, I visited New Orleans'),
# Document(page_content='In 2022, I visited New York'),
# Document(page_content='In 2023, I visited Paris')]
```
