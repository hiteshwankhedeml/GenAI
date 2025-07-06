# CrewAI - Demo

{% embed url="https://colab.research.google.com/drive/1SUyi4GolZJsKeQtj7lgFQlcWds2WxcjN?usp=sharing" %}

* Pick atleast 0.95 version, versions before that are compatable with pydantic 1
* Bydefault, CrewAI pickups OpenAI as llm ⇒ If we want to use any different LLM we can use&#x20;
  * from CrewAI import LLM
* **Note, we are not using any query in this demo.**

**Heirarchy:**

1. Crew
2. Agent
3. Process
4. Task&#x20;

**Steps:**

1. Define a agent ⇒ We can specify, role, goal, backstory, llm, allow delegation
2. Define task ⇒ We can specify expected\_output,&#x20;
3. Define crew
4. Kickoff

**Multi Agents:**

1. Define multiple agents ⇒ Writer, Reviewer
2. Define tasks ⇒ Gather information, Write Research Report, Review Report
3. Define crew
4. Kick off

*

```python
```
