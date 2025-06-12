# 🟢 Indexing

* From document we extract data
* Now we can create vector indexes in mongoDB, Postgres
* Lets assume PDF had 50 pages
* We divide data into chunks
* Suppose we have 10 chunks
* This 10 vectors we need to store in vector DB
* <mark style="color:purple;background-color:purple;">**When we store data in vectorDB we call it as indexing**</mark>
* <mark style="color:purple;background-color:purple;">**VectorDB does not have columns, it just has index**</mark>
* <mark style="color:purple;background-color:purple;">**For any query we will create embeddings and then do similarity search**</mark>

