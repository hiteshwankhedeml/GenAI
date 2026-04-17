---
hidden: true
---

# CrewAI - Human Input

* &#x20;

```python
from crewai import Agent, LLM

llm = LLM(model="gemini/gemini-2.0-flash")

# AI Researcher Agent
researcher_agent = Agent(
    role="Senior AI Researcher",
    goal="Discover and summarize the latest trends in AI and technology.",
    backstory="An expert in AI research who tracks emerging trends and their real-world applications.",
    verbose=True,
    allow_delegation=False,
    llm=llm
)

# AI Content Strategist Agent
content_strategist_agent = Agent(
    role="Tech Content Strategist",
    goal="Transform AI research insights into compelling blog content.",
    backstory="An experienced tech writer who makes AI advancements accessible to a broad audience.",
    verbose=True,
    allow_delegation=False,
    llm=llm
)

from crewai import Task

# Step 1: AI Research with Human Oversight
ai_research_task = Task(
    description=(
        "Conduct a deep analysis of AI trends in 2025. Identify key innovations, breakthroughs, and market shifts. "
        "Before finalizing, ask a human reviewer for feedback to refine the report."
    ),
    expected_output="A structured research summary covering AI advancements in 2025.",
    agent=researcher_agent,
    human_input=True  # Requires human validation before finalizing the answer
)

# Step 2: AI-Generated Blog Post with Human Review
blog_post_task = Task(
    description=(
        "Using insights from the AI Researcher, create an engaging blog post summarizing key AI advancements. "
        "Ensure the post is informative and accessible. Before finalizing, ask a human reviewer for approval."
    ),
    expected_output="A well-structured blog post about AI trends in 2025.",
    agent=content_strategist_agent,
    human_input=True  # Requires human approval before publishing
)

from crewai import Crew

ai_research_crew = Crew(
    agents=[researcher_agent, content_strategist_agent],  
    tasks=[ai_research_task, blog_post_task],  
    verbose=True,  
)

# Execute the workflow
result = ai_research_crew.kickoff()

# Display the final validated research output
print("\n=== Final AI Research Report ===")
print(result.raw)
```

```
╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: 7797ec85-eea7-4e8f-b326-cc3dd74af971                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
       Status: Executing Task...
    └── 🤖 Agent: Senior AI Researcher
            Status: In Progress
🚀 Crew: crew
└── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
       Status: Executing Task...
    └── 🤖 Agent: Senior AI Researcher
            Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
       Assigned to: Senior AI Researcher
       Status: ✅ Completed
    └── 🤖 Agent: Senior AI Researcher
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: a3be84ff-2859-4a04-b178-6aed8c65d574                                                                     │
│  Agent: Senior AI Researcher                                                                                    │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
├── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
│      Assigned to: Senior AI Researcher
│      Status: ✅ Completed
│   └── 🤖 Agent: Senior AI Researcher
│           Status: ✅ Completed
└── 📋 Task: fe099fb3-8a62-4701-994d-2756d99d7139
       Status: Executing Task...
🚀 Crew: crew
├── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
│      Assigned to: Senior AI Researcher
│      Status: ✅ Completed
│   └── 🤖 Agent: Senior AI Researcher
│           Status: ✅ Completed
└── 📋 Task: fe099fb3-8a62-4701-994d-2756d99d7139
       Status: Executing Task...
    └── 🤖 Agent: Tech Content Strategist
            Status: In Progress
🚀 Crew: crew
├── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
│      Assigned to: Senior AI Researcher
│      Status: ✅ Completed
│   └── 🤖 Agent: Senior AI Researcher
│           Status: ✅ Completed
└── 📋 Task: fe099fb3-8a62-4701-994d-2756d99d7139
       Status: Executing Task...
    └── 🤖 Agent: Tech Content Strategist
            Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: a3be84ff-2859-4a04-b178-6aed8c65d574
│      Assigned to: Senior AI Researcher
│      Status: ✅ Completed
│   └── 🤖 Agent: Senior AI Researcher
│           Status: ✅ Completed
└── 📋 Task: fe099fb3-8a62-4701-994d-2756d99d7139
       Assigned to: Tech Content Strategist
       Status: ✅ Completed
    └── 🤖 Agent: Tech Content Strategist
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: fe099fb3-8a62-4701-994d-2756d99d7139                                                                     │
│  Agent: Tech Content Strategist                                                                                 │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: 7797ec85-eea7-4e8f-b326-cc3dd74af971                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
from IPython.display import Markdown
Markdown(result.raw)
```
