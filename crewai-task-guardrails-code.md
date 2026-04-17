---
hidden: true
---

# CrewAI - Task Guardrails - Code

* &#x20;

```python
from crewai import Agent, LLM
from dotenv import load_dotenv
import os
load_dotenv()  # Load environment variables from .env file
#os.environ["OPENAI_API_KEY"] = os.getenv("GEMINI_API_KEY")


llm = LLM(model="gemini/gemini-2.0-flash", verbose=True, temperature=0.5,
          api_key=os.getenv("GEMINI_API_KEY"))
          
def validate_summary_length(task_output):
    try:
        print("Validating summary length")
        task_str_output = str(task_output)
        total_words = len(task_str_output.split())

        print(f"Word count: {total_words}")

        if total_words > 150:
            print("Summary exceeds 150 words")
            return (False, f"Summary exceeds 150 words. Word count: {total_words}")

        if total_words == 0:
            print("Summary is empty")
            return (False, "Generated summary is empty.")

        print("Summary is valid")
        return (True, task_output)

    except Exception as e:
        print("Validation system error")
        return (False, f"Validation system error: {str(e)}")

from crewai import Task, Agent

summary_agent = Agent(
    role="Summary Agent",
    goal="Summarize the research paper 'Convolutional Neural Networks' in 150 words.",
    backstory="You are a specialized agent that summarizes research papers.",
    verbose=True,
    llm=llm
)

summary_task = Task(
    description="Summarize a research paper in 150 words.",
    expected_output="A concise research summary 150 words.",
    agent=summary_agent,
    guardrail=validate_summary_length,

    max_retries=3
)

from crewai import Crew

summary_crew = Crew(
    agents=[summary_agent],
    tasks=[summary_task],
    verbose=True
)

result = summary_crew.kickoff()


```

```
Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: 94095776-5b51-47cb-bcc8-976b5d82ddd8                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: 411085d4-e001-475c-8061-e0298dbbdd2f
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: 411085d4-e001-475c-8061-e0298dbbdd2f
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: In Progress
# Agent: Summary Agent
## Task: Summarize a research paper in 150 words.


# Agent: Summary Agent
## Final Answer: 
Convolutional Neural Networks (CNNs) are a class of deep neural networks primarily used for image recognition and processing. Their architecture is inspired by the visual cortex and consists of convolutional layers, pooling layers, and fully connected layers. Convolutional layers use filters to detect local patterns, while pooling layers reduce dimensionality and computational complexity. CNNs excel at automatically learning spatial hierarchies of features from raw pixel data, making them robust to variations in position, scale, and orientation. They have achieved state-of-the-art performance in various tasks, including image classification, object detection, and image segmentation. CNNs are also applied in other fields like natural language processing and speech recognition, demonstrating their versatility and effectiveness in feature extraction and pattern recognition.


🚀 Crew: crew
└── 📋 Task: 411085d4-e001-475c-8061-e0298dbbdd2f
       Status: Executing Task...
    └── 🤖 Agent: Summary Agent
            Status: ✅ Completed
Validating summary length
Word count: 117
Summary is valid
🚀 Crew: crew
└── 📋 Task: 411085d4-e001-475c-8061-e0298dbbdd2f
       Assigned to: Summary Agent
       Status: ✅ Completed
    └── 🤖 Agent: Summary Agent
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: 411085d4-e001-475c-8061-e0298dbbdd2f                                                                     │
│  Agent: Summary Agent                                                                                           │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: 94095776-5b51-47cb-bcc8-976b5d82ddd8                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```
