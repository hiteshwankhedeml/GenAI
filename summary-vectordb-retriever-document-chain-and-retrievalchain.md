# 🟢 Summary: VectorDB, Retriever, Document Chain, and RetrievalChain

* <mark style="color:purple;background-color:purple;">**VectorDB stores embeddings and performs similarity search on documents.**</mark>
* <mark style="color:purple;background-color:purple;">**We convert VectorDB into a Retriever to get a flexible interface for fetching relevant documents.**</mark>
* <mark style="color:purple;background-color:purple;">**The Retriever fetches relevant documents for a given query.**</mark>
* <mark style="color:purple;background-color:purple;">**The Document Chain takes these documents + query and passes them to the LLM to generate a final answer.**</mark>
* <mark style="color:purple;background-color:purple;">**The RetrievalChain combines the Retriever and Document Chain into one unit that:**</mark>
  * <mark style="color:purple;background-color:purple;">**Takes a query,**</mark>
  * <mark style="color:purple;background-color:purple;">**Retrieves relevant documents,**</mark>
  * <mark style="color:purple;background-color:purple;">**Processes them with the LLM,**</mark>
  * <mark style="color:purple;background-color:purple;">**Returns the final output.**</mark>
