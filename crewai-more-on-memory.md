# CrewAI - More on memory

* &#x20;

```python
from crewai import Crew, Agent, Task, Process
from crewai.memory import ShortTermMemory
from crewai.memory.storage.rag_storage import RAGStorage

shared_db = "./short_term_memory.db"

storage = RAGStorage(path=shared_db, type="short_term")

short_term_memory = ShortTermMemory(storage=storage)
agent = Agent(role="A Personal Assistant",
              goal="""You are a personal assistant that can
                      help the user with their tasks.""",
              backstory="""You are a personal assistant that
                           can help the user with their tasks.""",
              verbose=True)

task = Task(description="Handle this task: {user_task}",
            expected_output="A clear and concise answer to the question.",
            agent=agent)


crew = Crew(
    agents=[agent], 
    tasks=[task], 
    process=Process.sequential,
    memory=True,
    short_term_memory=short_term_memory
)
crew.kickoff(inputs={"user_task": "My favorite color is #46778F."})
# Agent: A Personal Assistant
## Task: Handle this task: My favorite color is #46778F.


# Agent: A Personal Assistant
## Final Answer: 
Your favorite color is #46778F, which is a beautiful teal shade. This color can evoke feelings of calmness and serenity. It is often associated with nature, tranquility, and sophistication. If you have any specific applications or desired uses for this color, feel free to share!


CrewOutput(raw='Your favorite color is #46778F, which is a beautiful teal shade. This color can evoke feelings of calmness and serenity. It is often associated with nature, tranquility, and sophistication. If you have any specific applications or desired uses for this color, feel free to share!', pydantic=None, json_dict=None, tasks_output=[TaskOutput(description='Handle this task: My favorite color is #46778F.', name=None, expected_output='A clear and concise answer to the question.', summary='Handle this task: My favorite color is #46778F....', raw='Your favorite color is #46778F, which is a beautiful teal shade. This color can evoke feelings of calmness and serenity. It is often associated with nature, tranquility, and sophistication. If you have any specific applications or desired uses for this color, feel free to share!', pydantic=None, json_dict=None, agent='A Personal Assistant', output_format=<OutputFormat.RAW: 'raw'>)], token_usage=UsageMetrics(total_tokens=265, prompt_tokens=196, cached_prompt_tokens=0, completion_tokens=69, successful_requests=1))
user_query = "What is my favorite color?"

crew.kickoff(inputs={"user_task": user_query})
# Agent: A Personal Assistant
## Task: Handle this task: What is my favorite color?


# Agent: A Personal Assistant
## Final Answer: 
Your favorite color is #46778F, which is a beautiful teal shade. This color can evoke feelings of calmness and serenity. It is often associated with nature, tranquility, and sophistication. If you have any specific applications or desired uses for this color, feel free to share!


CrewOutput(raw='Your favorite color is #46778F, which is a beautiful teal shade. This color can evoke feelings of calmness and serenity. It is often associated with nature, tranquility, and sophistication. If you have any specific applications or desired uses for this color, feel free to share!', pydantic=None, json_dict=None, tasks_output=[TaskOutput(description='Handle this task: What is my favorite color?', name=None, expected_output='A clear and concise answer to the question.', summary='Handle this task: What is my favorite color?...', raw='Your favorite color is #46778F, which is a beautiful teal shade. This color can evoke feelings of calmness and serenity. It is often associated with nature, tranquility, and sophistication. If you have any specific applications or desired uses for this color, feel free to share!', pydantic=None, json_dict=None, agent='A Personal Assistant', output_format=<OutputFormat.RAW: 'raw'>)], token_usage=UsageMetrics(total_tokens=724, prompt_tokens=586, cached_prompt_tokens=0, completion_tokens=138, successful_requests=2))

```
