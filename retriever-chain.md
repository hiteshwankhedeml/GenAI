# Retriever Chain

* An interface to ask vector store anything
* <mark style="color:purple;background-color:purple;">**create retrieval chain will combine retriever and document chain together**</mark>
* We need to convert vector store into runnable binding using retriever
* Retriever can be added to any chain
* <mark style="color:purple;background-color:purple;">**Retriever chain is to get context from vectorDB, converting it into document and passing it to LLM  using document chain**</mark>
