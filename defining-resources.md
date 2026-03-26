# Defining resources

* Resources in MCP allow your server to expose information that can be directly included in prompts, rather than requiring tool calls to access data.&#x20;
* This creates a more efficient way to provide context to AI models.
* Resources in MCP servers allow you to expose data to clients, similar to GET request handlers in a typical HTTP server.&#x20;
* They're perfect for scenarios where you need to fetch information rather than perform actions.
* Let's say you want to build a document mention feature where users can type `@document_name` to reference files. This requires two operations:
  * Getting a list of all available documents (for autocomplete)
  * Fetching the contents of a specific document (when mentioned)
