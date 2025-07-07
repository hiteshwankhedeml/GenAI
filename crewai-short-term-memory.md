# CrewAI - Short term memory

* &#x20;Short term memory uses RAG

```python
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
    memory=True,
    verbose=True
)
user_input = """My favorite color is #46778F and
                my favorite Agent framework is CrewAI."""

result = crew.kickoff(inputs={"user_task": user_input})
╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: e0b9bdcd-8f00-4871-a4c7-81ec189b71aa                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Status: Executing Task...
    └── 🤖 Agent: Personal Assistant
            Status: In Progress
# Agent: Personal Assistant
## Task: Handle this task: My favorite color is #46778F and
                my favorite Agent framework is CrewAI.


# Agent: Personal Assistant
## Final Answer: 
Your favorite color is #46778F and your favorite agent framework is CrewAI.


🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Status: Executing Task...
    └── 🤖 Agent: Personal Assistant
            Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Assigned to: Personal Assistant
       Status: ✅ Completed
    └── 🤖 Agent: Personal Assistant
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e                                                                     │
│  Agent: Personal Assistant                                                                                      │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: e0b9bdcd-8f00-4871-a4c7-81ec189b71aa                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
user_input = "What is my favorite color?"

result = crew.kickoff(inputs={"user_task": user_input})
╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: e0b9bdcd-8f00-4871-a4c7-81ec189b71aa                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Status: Executing Task...
    └── 🤖 Agent: Personal Assistant
            Status: In Progress
# Agent: Personal Assistant
## Task: Handle this task: What is my favorite color?


# Agent: Personal Assistant
## Final Answer: 
Your favorite color is #46778F and your favorite Agent framework is CrewAI.


🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Status: Executing Task...
    └── 🤖 Agent: Personal Assistant
            Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e
       Assigned to: Personal Assistant
       Status: ✅ Completed
    └── 🤖 Agent: Personal Assistant
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: 449e3418-9ee5-4cb8-884e-8f2a2f506b0e                                                                     │
│  Agent: Personal Assistant                                                                                      │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: e0b9bdcd-8f00-4871-a4c7-81ec189b71aa                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
crew._short_term_memory
ShortTermMemory(embedder_config=None, crew=None, storage=<crewai.memory.storage.rag_storage.RAGStorage object at 0x125ecb5c0>)
user_input = "What is my favorite color?"

crew._short_term_memory.storage.search(user_input)
[{'id': '12276a9f-fd27-4c32-90a6-bb6a0c3ceffe',
  'metadata': {'agent': 'Personal Assistant',
   'observation': 'Handle this task: My favorite color is #46778F and\n                my favorite Agent framework is CrewAI.'},
  'context': 'I now can give a great answer  \nFinal Answer: Your favorite color is #46778F and your favorite Agent framework is CrewAI.',
  'score': 0.890435395010837},
 {'id': 'dc5f9303-a10c-405d-b66d-8dad334c9a86',
  'metadata': {'agent': 'Personal Assistant',
   'observation': 'Handle this task: What is my favorite color?'},
  'context': 'I now can give a great answer  \nFinal Answer: Your favorite color is #46778F and your favorite Agent framework is CrewAI.',
  'score': 0.890435395010837},
 {'id': '5125962c-dd48-4d8a-aa25-9888cda511ba',
  'metadata': {'agent': 'Personal Assistant',
   'observation': 'Handle this task: What is my favorite color?'},
  'context': 'I now can give a great answer  \nFinal Answer: Your favorite color is #46778F and your favorite Agent framework is CrewAI.',
  'score': 0.890435395010837}]
crew._long_term_memory.search(user_input)
user_input = "What is my favorite color?"

crew._entity_memory.search(user_input)
[{'id': 'ee229c69-b6da-4367-a010-6a968aa5f71a',
  'metadata': {'relationships': '- Identified as favorite color based on the task request.'},
  'context': 'Favorite Color(Color): #46778F, the color designated as the favorite.',
  'score': 0.6658871150305598},
 {'id': 'a32be6fd-1d0d-40bf-9dfb-da9490068380',
  'metadata': {'relationships': '- is related to personal preference'},
  'context': 'favorite color(color): A specific color that is preferred by someone.',
  'score': 0.7554879215777222},
 {'id': '89a74270-9af3-4c69-a93d-cfa81553160c',
  'metadata': {'relationships': '- is favorite color of the user'},
  'context': "favorite color(Color Code): The hexadecimal representation of the user's favorite color.",
  'score': 0.7812394006629673}]
```
