# Introduction to Hybrid Search



*

    <figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
* In RAG, when we query, it is converted into vector and then vector search is done in vectorDB
* Algorithm like cosine similarity is used for matching exact vector search
* Once we have response we combine it with our prompt template + LLM and then we get final output
* Here semantic search is done
* We also have something called as hybrid search
* We combine multiple search techniques
  * Semantic search ⇒ Dense vector search ⇒ Similar
  * Syntactic search ⇒ Exact search ⇒ Keyword search ⇒ Sparse vector
* VectorDB stores everything as dense vector
* Techniques such as OHE, BOW, TfIDF are sparse matrix
* Most of the e-commerece websites and all hybrid search is implemeted

**How does hybrid search work:**

* Documents will also be converted into sparse matrix during storing in vectorDB
* They will also be stores as dense vectors
* Whenever user puts queries, it will be divided into 2 types(sparse and dense) and then we will semantic search and keyword search result
*   Both of them are combined based on weightage

    <figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
