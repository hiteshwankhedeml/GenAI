# CrewAI - Entity memory

* &#x20;

```python
from crewai import Crew, Agent, Task, Process
from crewai.memory import EntityMemory
from crewai.memory.storage.rag_storage import RAGStorage

shared_db = "./agent1_entity_memory.db"

storage = RAGStorage(path=shared_db, type="short_term")

entity_memory = EntityMemory(storage=storage)
remembering_agent = Agent(role="Remembering Agent",
                          goal="""You are a assistant that can
                                  remember the user's information.""",
                          backstory="""You are a assistant that can remember
                                       the user's information.""",
                          verbose=True)
            
task = Task(description="Handle this task: {user_task}",
            expected_output="A clear and concise answer to the question.",
            agent=remembering_agent)

crew = Crew(
    agents=[remembering_agent], 
    tasks=[task], 
    process=Process.sequential,
    memory=True,
    entity_memory=entity_memory
)

crew.kickoff(inputs={"user_task": "My favorite color is #46778F."})
# Agent: Remembering Agent
## Task: Handle this task: My favorite color is #46778F.


# Agent: Remembering Agent
## Final Answer: 
Your favorite color is #46778F.


CrewOutput(raw='Your favorite color is #46778F.', pydantic=None, json_dict=None, tasks_output=[TaskOutput(description='Handle this task: My favorite color is #46778F.', name=None, expected_output='A clear and concise answer to the question.', summary='Handle this task: My favorite color is #46778F....', raw='Your favorite color is #46778F.', pydantic=None, json_dict=None, agent='Remembering Agent', output_format=<OutputFormat.RAW: 'raw'>)], token_usage=UsageMetrics(total_tokens=263, prompt_tokens=242, cached_prompt_tokens=0, completion_tokens=21, successful_requests=1))
crew._entity_memory.search("What is my favorite color?")
[{'id': 'b5e36392-36ff-41ba-bc17-83fab07482c3',
  'metadata': {'relationships': '- It is identified by the color code #46778F.'},
  'context': 'favorite color(color): The color that the person prefers, mentioned in the task.',
  'score': 0.9134150239774512}]
```
