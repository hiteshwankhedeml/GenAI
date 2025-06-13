# 🟢 Chain

* <mark style="color:purple;background-color:purple;">**A Chain is a modular pipeline that combines multiple components (like prompts, LLMs, retrievers) into a sequential flow.**</mark>
* <mark style="color:purple;background-color:purple;">**Chains manage how input data passes through each step to produce the final output.**</mark>
* Commonly used to structure complex workflows in LLM applications (e.g., question answering, summarization, translation).
* Enables reusability and easier debugging by breaking tasks into smaller steps.
* Types of chains include:
  * <mark style="color:purple;background-color:purple;">**LLMChain:**</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">Connects a prompt template with an LLM to generate output from input.</mark>
  * <mark style="color:purple;background-color:purple;">**SequentialChain:**</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">Runs multiple chains one after another, passing outputs along.</mark>
  * <mark style="color:purple;background-color:purple;">**RetrievalChain / RetrievalQAChain:**</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">Combines document retrieval with LLM answering.</mark>
* Chains can be customized to control the logic and data flow between components.
* Helps build scalable, maintainable applications with clear separation of concerns.
