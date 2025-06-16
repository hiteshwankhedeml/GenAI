# 🟢 ChatPromptTemplate vs PromptTemplate

* **ChatPromptTemplate:**
  * <mark style="color:purple;background-color:purple;">**Designed specifically for chat-based LLMs that expect a sequence of messages with roles (system, user, assistant).**</mark>
  * Structures prompts as a list of messages, each having a **role** and **content**.
  * Supports multi-turn conversations and role-based context.
  * Example use case: ChatGPT-style interactions or conversational agents.
* **PromptTemplate:**
  * <mark style="color:purple;background-color:purple;">**A simpler template for creating a single text prompt (string) with placeholders.**</mark>
  * <mark style="color:purple;background-color:purple;">**Does not organize content by message roles — just one block of text with variables to fill.**</mark>
  * Used for traditional LLMs or completion APIs that take plain text prompts.
  * Example use case: Single-turn prompts like “Translate this sentence: {text}”.
