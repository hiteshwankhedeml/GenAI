# CrewAI - Long Term memory

* &#x20;

```python
from crewai import Crew, Agent, Task, Process
from crewai.memory import LongTermMemory
from crewai.memory.storage.ltm_sqlite_storage import LTMSQLiteStorage
shared_db = "./agent_memory.db"

storage = LTMSQLiteStorage(db_path=shared_db)

long_memory = LongTermMemory(storage=storage)
agent1 = Agent(role="First Personal Assistant",
              goal="""You are a personal assistant that handles
                      user's tasks.""",
              backstory="""You are a personal assistant that
                           can help the user with their tasks.""",
              verbose=True)

task1 = Task(description="Handle this task: {user_task}",
            expected_output="A clear and concise answer to the question.",
            agent=agent1)

crew1 = Crew(
    agents=[agent1],
    tasks=[task1], 
    process=Process.sequential,
    memory=True,
    long_term_memory=long_memory
)
crew1.kickoff(inputs={"user_task": "My favorite color is #46778F."})
# Agent: First Personal Assistant
## Task: Handle this task: My favorite color is #46778F.


# Agent: First Personal Assistant
## Final Answer: 
Your favorite color is hex code #46778F, which represents a soothing and calming shade of blue-green. This particular color is known to evoke feelings of tranquility and peace, often associated with nature and serenity. Utilizing this color in your surroundings can create a harmonious atmosphere, enhancing relaxation and comfort in your space. Whether in fashion, interior design, or art, #46778F can be incorporated in various ways to bring a refreshing and calming aesthetic to any setting.


CrewOutput(raw='Your favorite color is hex code #46778F, which represents a soothing and calming shade of blue-green. This particular color is known to evoke feelings of tranquility and peace, often associated with nature and serenity. Utilizing this color in your surroundings can create a harmonious atmosphere, enhancing relaxation and comfort in your space. Whether in fashion, interior design, or art, #46778F can be incorporated in various ways to bring a refreshing and calming aesthetic to any setting.', pydantic=None, json_dict=None, tasks_output=[TaskOutput(description='Handle this task: My favorite color is #46778F.', name=None, expected_output='A clear and concise answer to the question.', summary='Handle this task: My favorite color is #46778F....', raw='Your favorite color is hex code #46778F, which represents a soothing and calming shade of blue-green. This particular color is known to evoke feelings of tranquility and peace, often associated with nature and serenity. Utilizing this color in your surroundings can create a harmonious atmosphere, enhancing relaxation and comfort in your space. Whether in fashion, interior design, or art, #46778F can be incorporated in various ways to bring a refreshing and calming aesthetic to any setting.', pydantic=None, json_dict=None, agent='First Personal Assistant', output_format=<OutputFormat.RAW: 'raw'>)], token_usage=UsageMetrics(total_tokens=547, prompt_tokens=442, cached_prompt_tokens=0, completion_tokens=105, successful_requests=1))
import sqlite3

conn = sqlite3.connect("agent_memory.db")
cursor = conn.cursor()
cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")

tables = cursor.fetchall()

print("Tables in the database:")
for table in tables:
    print(table[0])

cursor.execute("SELECT * FROM long_term_memories;")
rows = cursor.fetchall()

for row in rows:
    print(row)
Tables in the database:
long_term_memories
sqlite_sequence
(1, 'Handle this task: My favorite color is #46778F.', '{"suggestions": ["Provide a more direct answer to the question", "Limit the response to the specific color description without added commentary", "Ensure to stick closely to the expected output format", "Avoid overly elaborate explanations that may detract from the core response"], "quality": 6.0, "agent": "First Personal Assistant", "expected_output": "A clear and concise answer to the question."}', '1744540942.912284', 6.0)
task2 = Task(description="Handle this task: {user_task}",
            expected_output="A clear and concise answer to the question.",
            agent=agent1)

crew2 = Crew(
    agents=[agent1], 
    tasks=[task2], 
    process=Process.sequential,
    memory=True,
    long_term_memory=long_memory
)

crew2.kickoff(inputs={"user_task": "What is my favorite color?"})
# Agent: First Personal Assistant
## Task: Handle this task: What is my favorite color?


# Agent: First Personal Assistant
## Final Answer: 
Your favorite color is hex code #46778F, which is a soothing and calming shade of blue-green. This specific color often evokes feelings of tranquility and is commonly associated with nature and serenity, making it a perfect choice for creating a peaceful atmosphere in any environment.


CrewOutput(raw='Your favorite color is hex code #46778F, which is a soothing and calming shade of blue-green. This specific color often evokes feelings of tranquility and is commonly associated with nature and serenity, making it a perfect choice for creating a peaceful atmosphere in any environment.', pydantic=None, json_dict=None, tasks_output=[TaskOutput(description='Handle this task: What is my favorite color?', name=None, expected_output='A clear and concise answer to the question.', summary='Handle this task: What is my favorite color?...', raw='Your favorite color is hex code #46778F, which is a soothing and calming shade of blue-green. This specific color often evokes feelings of tranquility and is commonly associated with nature and serenity, making it a perfect choice for creating a peaceful atmosphere in any environment.', pydantic=None, json_dict=None, agent='First Personal Assistant', output_format=<OutputFormat.RAW: 'raw'>)], token_usage=UsageMetrics(total_tokens=1025, prompt_tokens=855, cached_prompt_tokens=0, completion_tokens=170, successful_requests=2))
```
