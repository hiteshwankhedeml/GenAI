---
hidden: true
---

# ✈️ Graph RAG - Code

* &#x20;Need to understand edges and strategy used here

```python
!pip install langchain-graph-retriever
!pip install graph_rag_example_helpers
!pip install langchain_openai

from graph_rag_example_helpers.datasets.animals import fetch_documents

animals=fetch_documents()

animals
# [Document(id='aardvark', metadata={'type': 'mammal', 'number_of_legs': 4, 
# 'keywords': ['burrowing', 'nocturnal', 'ants', 'savanna'],
# 'habitat': 'savanna', 'tags': [{'a': 5, 'b': 7}, {'a': 8, 'b': 10}]},
# page_content='the aardvark is a nocturnal mammal known for its burrowing 
# habits and long snout used to sniff out ants.'),
# Document(id='albatross', metadata={'type': 'bird', 'number_of_legs': 2,
# 'keywords': ['seabird', 'wingspan', 'ocean'], 'habitat': 'marine',
# 'tags': [{'a': 5, 'b': 8}, {'a': 8, 'b': 10}]},
# page_content='the albatross is a large seabird with the longest wingspan
# of any bird, allowing it to glide effortlessly over oceans.'),
# Document(id='alligator', metadata={'type': 'reptile', 'number_of_legs': 4, 
#'keywords': ['reptile', 'jaws', 'wetlands'], 'diet': 'carnivorous',
# 'nested': {'a': 5}}, page_content='alligators are large
# reptiles with powerful jaws and are commonly found in
# freshwater wetlands.'),

len(animals) # 99

animals[0].metadata
# {'type': 'mammal',
# 'number_of_legs': 4,
# 'keywords': ['burrowing', 'nocturnal', 'ants', 'savanna'],
# 'habitat': 'savanna',
# 'tags': [{'a': 5, 'b': 7}, {'a': 8, 'b': 10}]}

import getpass
import os
if not os.environ.get("OPENAI_API_KEY"):
  os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter API key for OpenAI: ")
  
from langchain_openai import OpenAIEmbeddings

embeddings=OpenAIEmbeddings(model="text-embedding-3-large")

from langchain_core.vectorstores import InMemoryVectorStore

vector_store=InMemoryVectorStore.from_documents(
    documents=animals,
    embedding=embeddings
)

from graph_retriever.strategies import Eager
from langchain_graph_retriever import GraphRetriever

traversal_retriever = GraphRetriever(
    store = vector_store,
    edges = [("habitat", "habitat"), ("origin", "origin")],
    strategy = Eager(k=5, start_k=1, max_depth=2),
)

traversal_retriever

results=traversal_retriever.invoke("what animal could be found near a anaconda?")

len(results) #

results[0]
# Document(id='iguana', metadata={'_depth': 0, 
# '_similarity_score': np.float64(0.39788762643161923), 'type': 'reptile', 
# 'number_of_legs': 4, 'keywords': ['large lizard', 'herbivore', 'basking'],
# 'habitat': 'forest'}, page_content='iguanas are large herbivorous lizards
# often found basking in trees and near water.')

from langchain.chat_models import init_chat_model

llm = init_chat_model("gpt-4o-mini", model_provider="openai")

from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough

prompt = ChatPromptTemplate.from_template(
"""Answer the question based only on the context provided.

Context: {context}

Question: {question}"""
)

def format_docs(docs):
    return "\n\n".join(f"text: {doc.page_content} metadata: {doc.metadata}" for doc in docs)

chain = (
    {"context": traversal_retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

chain.invoke("what animal could be found near a anaconda?")
# Based on the context provided, iguanas, which are large herbivorous lizards often found near water, could be found near an anaconda.


```
