# Types of Indexing

1. **Flat Index (Brute force search)**

* Brute force means exact search
* It will go through all vectors and find similarity score between query and the vector
* It will pick the most similar vector
* Slow process

2. **IVF ( Cluster based technique) - Inverted file index**

* We will be creating cluster of vectors based on similarity
* Here we will find similarity between centroid and query
* We use approximate search here

3. **HNSW (Graph based) - Hierarchical navigable small world graph**

*

4. LSH (Hashing based) - Local sensitivity hashing
