# 🟢 MCP

* Think of MCP (Model Context Protocol) as a plug socket, not the appliance
* The LLM is the appliance that needs power (data, tools, context)
* MCP defines how the plug fits, not what the appliance does
* <mark style="color:purple;background-color:purple;">**Without MCP**</mark>
  * <mark style="color:purple;background-color:purple;">**Every app connects LLM to tools differently**</mark>
  * <mark style="color:purple;background-color:purple;">**Custom code for DB, API, files each time**</mark>
  * <mark style="color:purple;background-color:purple;">**Tight coupling, hard to change or audit**</mark>
* <mark style="color:purple;background-color:purple;">**With MCP**</mark>
  * <mark style="color:purple;background-color:purple;">**Tools expose themselves in a standard way**</mark>
  * <mark style="color:purple;background-color:purple;">**LLM asks: “What tools are available?”**</mark>
  * <mark style="color:purple;background-color:purple;">**LLM calls tools using a fixed protocol**</mark>
  * <mark style="color:purple;background-color:purple;">**No custom glue logic per app**</mark>
* **Concrete example**
  * Question: “Show my last 3 payslips”
  * MCP tool advertises: `get_payslips(employee_id)`
  * LLM selects the tool
  * MCP executes it and returns structured data
  * LLM converts data into natural language
* **What MCP is NOT**
  * Not a database
  * Not a vector store
  * Not an agent
  * Not a model
* **Why interviewers care**
  * Enables agentic AI
  * Makes tool usage safe and standard
  * Decouples LLMs from enterprise systems
* <mark style="color:purple;background-color:purple;">**One-line interview explanation**</mark>
  * <mark style="color:purple;background-color:purple;">**MCP is a standard way for LLMs to discover and use external tools and data without hard-coded integrations**</mark>
