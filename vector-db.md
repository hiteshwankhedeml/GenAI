# 🟢 Vector DB

* Backbone of RAG
* We can configure on system, or over cloud as well
* <mark style="color:purple;background-color:purple;">**VectorDB holds the vector**</mark>
* SQL based DB used for tabular data ⇒ We use SQL queries to get data
* There are NoSQL DB like mongoDB
* Vector is a set of number
* \[ 0.1, 0.5. 0.3, 0.7. 0.9. 2 ] ⇒ Dimension 6
* \[ 0.1 0.5 ] ⇒ Dimension 2
* \[\[       ]] ⇒ Multi dimensional vector
* \[\[\[     ]]] ⇒ 3D vector
* In case of image we have Name, type, size, embedding
* If we want to store it in SQL DB then we will store data in different columns
* If for a new image, we want to find similar images
* But SQL DB does not support similarity search
* VectorDB is optimized for search
* <mark style="color:purple;background-color:purple;">**VectorDB provides**</mark>
  * <mark style="color:purple;background-color:purple;">**Indexing**</mark>
  * <mark style="color:purple;background-color:purple;">**Searching**</mark>
