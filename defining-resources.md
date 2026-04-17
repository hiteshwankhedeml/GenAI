# 🟢 Defining resources

* <mark style="color:$danger;background-color:purple;">**Resources in MCP allow your server to expose information that can be directly included in prompts, rather than requiring tool calls to access data.**</mark>&#x20;
* This creates a more efficient way to provide context to AI models.
* Resources in MCP servers allow you to expose data to clients, similar to GET request handlers in a typical HTTP server.&#x20;
* They're perfect for scenarios where you need to fetch information rather than perform actions.
* Let's say you want to build a document mention feature where users can type `@document_name` to reference files. <mark style="color:purple;background-color:purple;">**This requires two operations:**</mark>
  * <mark style="color:purple;background-color:purple;">**Getting a list of all available documents (for autocomplete)**</mark>
  * <mark style="color:purple;background-color:purple;">**Fetching the contents of a specific document (when mentioned)**</mark>

