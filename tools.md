# Tools

* Tools are useful whenever you want a model to interact with external systems.
* External systems (e.g., APIs) often require a particular input schema or payload, rather than natural language.
* When we bind an API, for example, as a tool we given the model awareness of the required input schema.
* The model will choose to call a tool based upon the natural language input from the user.
* And, it will return an output that adheres to the tool's schema.
* Many LLM providers support tool calling and tool calling interface in LangChain is simple.
* You can simply pass any Python function into ChatModel.bind\_tools(function).
