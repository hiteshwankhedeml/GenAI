# 🟢 Embedding Models - OpenAI

* Open AI embedding models - [https://platform.openai.com/docs/models](https://platform.openai.com/docs/models)
* <mark style="color:purple;background-color:purple;">**We can regulate the model dimension by passing the parameter dimension**</mark>
* OpenAI embeddings are paid

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">Initialize the embedding object ⇒ Pass model and can also pass dimensions optionally</mark>
* <mark style="color:purple;background-color:purple;">embed\_query(text) ⇒ To embed text</mark>
* <mark style="color:purple;background-color:purple;">embed\_documents(documents) ⇒ To embed documents</mark>
* <mark style="color:purple;background-color:purple;">Can use cosine similarity on embedded text as well</mark>

```python
from langchain_openai import OpenAIEmbeddings

openai_embedding=OpenAIEmbeddings(model='text-embedding-3-large')
result=openai_embedding.embed_query("India is a growing country")
len(result) # 3072

openai_embedding_small=OpenAIEmbeddings(model='text-embedding-3-small')
result2=openai_embedding_small.embed_query("India is a growing country")
len(result2) # 1536

openai_embedding_64=OpenAIEmbeddings(model='text-embedding-3-large',dimensions=64)
result3=openai_embedding_64.embed_query("India is a growing country")
len(result3) # 64

documents=["what is a capital of USA",
           "who is a president of usa",
           "who is a prime minister of india"]
result=openai_embedding.embed_documents(documents)
len(result) # 3
result[0] # to check individual embedding of document
```

* **Similarity**

```python
documents=["what is a capital of USA",
           "who is a president of usa",
           "who is a prime minister of india"]

my_query="narendra modi is indian prime minister"

query_embedding=openai_embedding.embed_query(my_query)
document_embedding=openai_embedding.embed_documents(documents)

from sklearn.metrics.pairwise import cosine_similarity

scores=cosine_similarity([query_embedding],document_embedding)
scores
# array([[0.13453207, 0.38540264, 0.69040144]])
```
