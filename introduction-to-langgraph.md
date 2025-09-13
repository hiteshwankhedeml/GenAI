# 🟢 Introduction to LangGraph

**Langchain Advantages:**

* Amazing library ⇒ for component, core, integration
* Very easy to create applications



<mark style="color:purple;background-color:purple;">**Complex workflow:**</mark>&#x20;

* LLM calls tools (tools was converted to agents), this is how we did all tasks
* In GenAI we are all able to create message history, RAG, tools, agents
* <mark style="color:purple;background-color:purple;">**For complex workflow like Plan a 5 days to paris ⇒ This is a complex workflow**</mark>
* <mark style="color:purple;background-color:purple;">**Day 1 ⇒ Travelling**</mark>
* <mark style="color:purple;background-color:purple;">**Day 2 ⇒ Sighseeing 1, 2**</mark>
* <mark style="color:purple;background-color:purple;">**Day 5 ⇒ Flyback**</mark>
* Or if we ask Plan a 5 days trip to paris keeping price in mind ⇒ This is more complex workflow
* In such task LLM will be brain and it will try to use different tools to come to decision
* <mark style="color:purple;background-color:purple;">**Here state management is also required ⇒ Whatever is planned on day 1, should also be communicated to next day and so on**</mark>



LangGraph:

* Used for building stateful, multi actor application with LLMs
* Agents can also communicate with each other
* Each agent can perform different tasks
* Benefits:
  * Cycle controllability&#x20;
  * Persistence
* It allows to define flows that involve cycles, essential for most agentic architectures, differentiating it from DAG-based solutions.
* Using LangGraph studio we can develop all this workflows using drag and drop
* Using graphDB we can develop graph knowledge
* Graph will be static here



**Why LangGraph?**

* Simplifies the development
  * State management of the agent and agent coordination
    * Agent1: Google search
    * Agent2: Wikipedia search
    * Agent3: VectorDB search
    * Chatbot using all this multi AI agents
    * In such application we have to do statement for all the agents and also agent coordination
  * We need to define workflow, logics without worrying about all this
* Flexibility:
  * Flexibility to define own agents logic and communication protocol
  * This allows highly customized application tailored to use case
* Scalability:
  * We can build large scale multi agent applications, which can handle large volume of interaction
* Fault tolerance:
  * Handle errors, Reliability
* Entire workflow is converted into graphs
* Between start to end it is one application and in between we can create any number of nodes
* It shows the flow
* Nodes can be any LLM or function calling it can also be tool calling function
* Condtional nodes which node to go to
* In graph on the right, when user gives query ⇒ Goes to Node 1 ⇒ Node 1 decides whether to go to Node 2 or Node 3 ⇒ We call this as conditional edges
* Whenever we got from one node to another ⇒ State also changes
* Basic Agentic AI will be having start node, then LLM which will be brain, and LLM will be deciding which tool to be used
* If we make the tool response go back to LLM, then it means LLM is deciding who should do what and once it is done, it again decide what needs to be done next
* So here LLM will be React(Reasoning and acting)
*

    <figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
