# ✈️ RAG Architecture

* If chunking is required, then we do it
* We embed and store in vectorDB
* When user asks we dont directly pass it to LLM
* We embed and pass it to vectorDB and retrieve relevant information
* We rank it and then this is passed to LLM and output is generated
* Data collection to storing of embedding data in vectorDB is known as Data Ingestion
* Retrieval is done to provide relevant information to the LLM (Query + Embedding + Similarity search)
*

```
<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>
```
