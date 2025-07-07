# CrewAI - Memory

* &#x20;[https://github.com/sourangshupal/crewai-advanced/blob/main/memory.ipynb](https://github.com/sourangshupal/crewai-advanced/blob/main/memory.ipynb)

```python
import nest_asyncio # Required only in jupyter notebook

nest_asyncio.apply()
from crewai import Crew, Agent, Task, Process

# Define a simple agent (assuming a default LLM agent)
assistant = Agent(role="Personal Assistant",
                  goal="""You are a personal assistant that can
                          help the user with their tasks.""",
                  backstory="""You are a personal assistant that
                               can help the user with their tasks.""",
                  verbose=True)

task = Task(description="Handle this task: {user_task}",
            expected_output="A clear and concise answer to the question.",
            agent=assistant)

# Create a crew with memory enabled
crew = Crew(
    agents=[assistant], 
    tasks=[task], 
    process=Process.sequential,
    verbose=True
)
user_input = """My favorite color is #46778F and
                my favorite Agent framework is CrewAI."""

result = crew.kickoff(inputs={"user_task": user_input})
user_input = "What is my favorite color?"

result = crew.kickoff(inputs={"user_task": user_input})
```
