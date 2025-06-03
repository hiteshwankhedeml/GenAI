# Chain

* <mark style="color:purple;background-color:purple;">**A Chain is a modular pipeline that combines multiple components (like prompts, LLMs, retrievers) into a sequential flow.**</mark>
* Chains manage how input data passes through each step to produce the final output.
* Commonly used to structure complex workflows in LLM applications (e.g., question answering, summarization, translation).
* Enables reusability and easier debugging by breaking tasks into smaller steps.
* Types of chains include:
  * **LLMChain:** Connects a prompt template with an LLM to generate output from input.
  * **SequentialChain:** Runs multiple chains one after another, passing outputs along.
  * **RetrievalChain / RetrievalQAChain:** Combines document retrieval with LLM answering.
* Chains can be customized to control the logic and data flow between components.
* Helps build scalable, maintainable applications with clear separation of concerns.
