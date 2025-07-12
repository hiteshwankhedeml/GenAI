# Key Elements of AI Agents

1. Roleplaying

* The ability for them to perform or take a shape or function impact the responses
*   Using the correct keywords while setting goals, roles and backstory will help get better response

    <figure><img src=".gitbook/assets/{9916DB38-7D82-4BC0-A50A-37B200AB21EA}.png" alt=""><figcaption></figcaption></figure>

2. **Focus:**

* if we mix things too much, like too much tools or information or too much context, the model might loose important information
* Do not rely on one agent to do it all, better to use multi agents

3. **Tools:**

* Giving too many tools for agents can get confusing
* They might not end up using correct tool
* Provide only the key tools to the agent

4. Cooperation

* Ability to cooperate and bounce ideas to each other gives good results
* Take feedbacks
* Delegate feedbacks

5. Guardrails

* AI applications have fuzzy inputs, fuzzy outputs
* But we don't want hallucination, or agent stuck on repeated loops
* In 1st version of CrewAI it used to happen
* Guardrails helps agent from derailing and stack on track

6. Memory

* Memory can make a huge difference on agents
* Memory helps agent remember what it has done&#x20;
* The ability to recollect what it has done in the past, learn from it and apply their knowledge into future execution
* In CrewAI agents will have&#x20;
  * Short term memory&#x20;
    * Memory during execution of task
    * Share knowledge, activities and learnings with other agents
    * Share intermediate information even before providing "task completion" output
  * Long term memory:
    * Remember and learn
    * Memory stored after execution of current tasks
    * Stored in DB
    * Can be used in any future tasks
    * Leads to self improving agents
  * Entity memory:
    * It also short lived
    * It stores what are the subjects which are being discussed
    *

        <figure><img src=".gitbook/assets/{FEF20043-9834-4A39-86B3-ACD57167CE5A}.png" alt=""><figcaption></figcaption></figure>

