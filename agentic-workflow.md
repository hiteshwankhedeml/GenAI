# Agentic Workflow

* We give input to LLM and get output, using this suppose we have created AI assistant application
* But LLM might generate a wrong response, as it might not have been trained on latest/domain specific data
* In that case, we can create LLM with some knowledge base
* It will take context from knowledge base and then give output (RAG)
* Then this AI assistant will be called RAG based AI assistant
* We can think of LLM as artificial brain
* Function of brain is to give instructions and based on it actions should be taken
* Suppose we ask LLM to draft a mail and send it to someone
* So LLM will 1st think and generate mail content
* Then LLM will take action by tool calling to send mail
* After sending it will again observe if it has done correct action or not
* This flow is called Agentic flow
* To develop this Agentic AI laggraph has been developed
* There are other frameworks like CrewAI, Agno etc
* Suppose we have multiple tools binded to LLM
* Based on query, LLM will take decision which tool needs to be called
