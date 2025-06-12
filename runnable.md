# 🟢 Runnable

* <mark style="color:purple;background-color:purple;">**A runnable is any component that can be executed like a function using methods such as**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`.invoke()`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`.batch()`**</mark><mark style="color:purple;background-color:purple;">**, or**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`.stream()`**</mark><mark style="color:purple;background-color:purple;">**. which can be used in the chain**</mark>
* Common runnables include prompt templates, models, chains, and output parsers.
* A **retriever** is a wrapper around a vector database that exposes a standard method `get_relevant_documents(query)`.
* LangChain chains like `RetrievalQA` require retrievers, not raw vector databases.
* Retrievers allow additional features like `top_k` control, metadata filtering, and custom logic.
* <mark style="color:purple;background-color:purple;">**You convert a vector store into a retriever using**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`vectorstore.as_retriever()`**</mark><mark style="color:purple;background-color:purple;">**.**</mark>
* This enables compatibility and integration with chains and agents.
* `get_relevant_documents()` is not added directly to vector databases to maintain separation of concerns.
* VectorDBs focus purely on vector storage and similarity search operations.
* Retrievers act as an abstraction layer, allowing interchangeable retrieval strategies (e.g., BM25, hybrid).
* This design allows for cleaner APIs and better maintainability.
* <mark style="color:purple;background-color:purple;">**Separating retrieval logic from storage also enables easier testing, plugging, and swapping components in LangChain.**</mark>
