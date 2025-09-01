# ✈️ Working with VectorStore and Retriever

* These abstractions are designed to support retrieval of data-- from (vector) databases and other sources-- for integration with LLM workflows.
* They are important for applications that fetch data to be reasoned over as part of model inference, as in the case of retrieval-augmented generation.

**Document:**

* LangChain implements a Document abstraction, which is intended to represent a unit of text and associated metadata. It has two attributes:
* page\_content: a string representing the content;
* metadata: a dict containing arbitrary metadata. The metadata attribute can capture information about the source of the document, its relationship to other documents, and other information. Note that an individual Document object often represents a chunk of a larger document

**Retrievers:**

* LangChain VectorStore objects do not subclass Runnable, and so cannot immediately be integrated into LangChain Expression Language chains.
* LangChain Retrievers are Runnables, so they implement a standard set of methods (e.g., synchronous and asynchronous invoke and batch operations) and are designed to be incorporated in LCEL chains.
* We can create a simple version of this ourselves, without subclassing Retriever. If we choose what method we wish to use to retrieve documents, we can create a runnable easily. Below we will build one around the similarity\_search method:
* Vectorstores implement an as\_retriever method that will generate a Retriever, specifically a VectorStoreRetriever.
* These retrievers include specific search\_type and search\_kwargs attributes that identify what methods of the underlying vector store to call, and how to parameterize them.

```python
from langchain_core.documents import Document

documents = [
    Document(
        page_content="Dogs are great companions, known for their loyalty and friendliness.",
        metadata={"source": "mammal-pets-doc"},
    ),
    Document(
        page_content="Cats are independent pets that often enjoy their own space.",
        metadata={"source": "mammal-pets-doc"},
    ),
    Document(
        page_content="Goldfish are popular pets for beginners, requiring relatively simple care.",
        metadata={"source": "fish-pets-doc"},
    ),
    Document(
        page_content="Parrots are intelligent birds capable of mimicking human speech.",
        metadata={"source": "bird-pets-doc"},
    ),
    Document(
        page_content="Rabbits are social animals that need plenty of space to hop around.",
        metadata={"source": "mammal-pets-doc"},
    ),
]

import os
from dotenv import load_dotenv
load_dotenv()
from langchain_groq import ChatGroq
groq_api_key=os.getenv("GROQ_API_KEY")

os.environ["HF_TOKEN"]=os.getenv("HF_TOKEN")

llm=ChatGroq(groq_api_key=groq_api_key,model="Llama3-8b-8192")
llm

from langchain_huggingface import HuggingFaceEmbeddings
embeddings=HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")

## VectorStores
from langchain_chroma import Chroma

vectorstore=Chroma.from_documents(documents,embedding=embeddings)
vectorstore

vectorstore.similarity_search("cat")

## Async query
await vectorstore.asimilarity_search("cat")
#[Document(page_content='Cats are independent pets that often enjoy 
#      their own space.', metadata={'source': 'mammal-pets-doc'}),
# Document(page_content='Dogs are great companions, known for their loyalty and 
#      friendliness.', metadata={'source': 'mammal-pets-doc'}),
# Document(page_content='Rabbits are social animals that need plenty of space 
#      to hop around.', metadata={'source': 'mammal-pets-doc'}),
# Document(page_content='Parrots are intelligent birds capable of mimicking 
#      human speech.', metadata={'source': 'bird-pets-doc'})]

# vectorstore.similarity_search_with_score("cat")
# #[(Document(page_content='Cats are independent pets that often enjoy 
#   their own space.', metadata={'source': 'mammal-pets-doc'}),
#    0.9351058006286621),
#(Document(page_content='Dogs are great companions, known for their loyalty
#    and friendliness.', metadata={'source': 'mammal-pets-doc'}),
#     1.5740901231765747),
# (Document(page_content='Rabbits are social animals that need plenty of space 
#     to hop around.', metadata={'source': 'mammal-pets-doc'}),
#     1.595690131187439),
#(Document(page_content='Parrots are intelligent birds capable of mimicking 
#     human speech.', metadata={'source': 'bird-pets-doc'}),
#     1.665792465209961)]


from typing import List

from langchain_core.documents import Document
from langchain_core.runnables import RunnableLambda

# Below is not recommended approach
retriever=RunnableLambda(vectorstore.similarity_search).bind(k=1)
retriever.batch(["cat","dog"])

retriever=vectorstore.as_retriever(
    search_type="similarity",
    search_kwargs={"k":1}
)
retriever.batch(["cat","dog"])

## RAG
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough

message = """
Answer this question using the provided context only.

{question}

Context:
{context}
"""
prompt = ChatPromptTemplate.from_messages([("human", message)])

rag_chain={"context":retriever,"question":RunnablePassthrough()}|prompt|llm

response=rag_chain.invoke("tell me about dogs")
print(response.content)
# According to the provided context, dogs are great companions, 
#                     known for their loyalty and friendliness.
```
