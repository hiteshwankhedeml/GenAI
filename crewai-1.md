# CrewAI

* Light Packaging ⇒ UV installer ⇒ Built on rust
* LangGraph needs about 1.5GB for installing whereas this needs about 400MB
* Built on top of pydantic v2
* Suppose we have an Agent 1 and we want to assign task1 to it
* Task can be of 2 types — LLM calls OR tools
* There are lots of LLM providers
* Types of tools
  * Prebuilt in framework
  * Third party tools - Like salesforce etc
  * API calls - RESP API - Used mostly with custom tools
* If more granuality or control needed then use langChain, otherwise CrewAI / Agno
