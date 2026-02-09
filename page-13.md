# 🟢 Introduction to Hybrid Search



*

    <figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
* In RAG, when we query, it is converted into vector and then vector search is done in vectorDB
* Algorithm like cosine similarity is used for matching exact vector search
* Once we have response we combine it with our prompt template + LLM and then we get final output
* <mark style="color:purple;background-color:purple;">**Normally for searching relevant document from vectorDB, semantic search is used**</mark>
* <mark style="color:purple;background-color:purple;">**Semantic search = searching based on meaning, not just exact keyword matches.**</mark>
* <mark style="color:purple;background-color:purple;">**In hybrid search we combine multiple search techniques**</mark>
  * <mark style="color:purple;background-color:purple;">**Semantic search ⇒ Dense vector search ⇒ Similar**</mark>
  * <mark style="color:purple;background-color:purple;">**Syntactic search ⇒ Exact search ⇒ Keyword search ⇒ Sparse vector**</mark>
* <mark style="color:purple;background-color:purple;">**VectorDB stores everything as dense vector**</mark>
* <mark style="color:purple;background-color:purple;">**Techniques such as OHE, BOW, TfIDF are sparse matrix**</mark>
* Most of the e-commerece websites and all hybrid search is implemeted

**How does hybrid search work:**

* <mark style="color:purple;background-color:purple;">**Documents will also be converted into sparse matrix + dense matrix during storing in vectorDB**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**Whenever user puts queries, it will be divided into 2 types(sparse and dense) and then we will semantic search and keyword search result**</mark>
*   <mark style="color:purple;background-color:purple;">**Both of them are combined based on weightage**</mark>

    <figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
