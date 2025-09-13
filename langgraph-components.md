# LangGraph Components

* &#x20;Prompt templates: Allows reusable prompts
* LangChain community contains 100s of tools
* New in LangGraph:
  * Cyclic Graphs
  * Persistence
  * Human-in-the-loop
* LangGraph is an extension of LangChain that supports graphs
* Single and multi agents flows are described and respresented as graphs
* Allows for extremely controlled flows
* Built-in persistence allows for human in the loop workflows

**Core Concepts**

* Nodes: Agents or functions
* Edges: connect nodes
* Conditional edges: Decisions
*

    <figure><img src=".gitbook/assets/{6974601E-7B9D-4780-B5DD-6F70729DBF68}.png" alt=""><figcaption></figcaption></figure>
* Agent state is accessible to all parts of the graph
* It is local to the graph
* Can be stored in a persistent layer
*

    <figure><img src=".gitbook/assets/{84555497-D3BA-4FBC-8DF1-FE1F4C167577}.png" alt=""><figcaption></figcaption></figure>
