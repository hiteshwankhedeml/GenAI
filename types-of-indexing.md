# 🟢 Types of Indexing

1. **Flat Index (Brute force search)**

* <mark style="color:purple;background-color:purple;">**Brute force means exact search**</mark>
* <mark style="color:purple;background-color:purple;">**It will go through all vectors and find similarity score between query and the vector**</mark>
* It will pick the most similar vector
* Slow process

2. **IVF ( Cluster based technique) - Inverted file index**

* <mark style="color:purple;background-color:purple;">**We will be creating cluster of vectors based on similarity**</mark>
* <mark style="color:purple;background-color:purple;">**Here we will find similarity between centroid and query**</mark>
* We use approximate search here

3. **HNSW (Graph based) - Hierarchical navigable small world graph**

*

4. LSH (Hashing based) - Local sensitivity hashing
