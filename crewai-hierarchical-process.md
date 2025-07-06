# CrewAI - Hierarchical Process

* &#x20;

```python
from crewai import Agent, LLM

llm = LLM(model="gpt-4o", api_key=os.environ["OPENAI_API_KEY"])

# research a new project idea, do the research on market demand, risk, and potential return on investment.

# Define the Manager AI
manager_agent = Agent(
    role="Project Research Manager",
    goal="Oversee the project research and ensure timely, high-quality responses.",
    backstory="""An experienced project manager responsible
                 for ensuring project research.""",
    allow_delegation=True,
    verbose=True,
    llm=llm
)

# Define the Technical Support AI
market_demand_agent = Agent(
    role="Market Demand Analyst",
    goal="Write market demand content.",
    backstory="""A skilled market demand analyst who
                 writes market demand content.""",
    allow_delegation=False, 
    verbose=True,
    llm=llm
)

# Define the Fiction Writer AI
risk_analysis_agent = Agent(
    role="Risk Analysis Analyst",
    goal="Write risk analysis content.",
    backstory="""A risk analysis analyst who
                 writes fiction content.""",
    allow_delegation=False, 
    verbose=True,
    llm=llm
)

# Define the Fiction Writer AI
return_on_investment_agent = Agent(
    role="Return on Investment Analyst",
    goal="Write return on investment content.",
    backstory="""A return on investment analyst who
                writes return on investment content.""",
    allow_delegation=False, 
    verbose=True,
    llm=llm
)

from crewai import Task

manager_task = Task(
    description="""Oversee the project research on {project_title} and ensure timely, high-quality responses.""",
    expected_output="A manager-approved response ready to be sent as an article on {project_title}.",
    agent=manager_agent, 
)

market_demand_task = Task(
    description="""Analyze the market demand for the project title '{project_title}'""",
    expected_output="A categorized project title labeled as 'Technical' or 'Fiction'.",
    agent=market_demand_agent, 
)

risk_analysis_task = Task(
    description="""Analyze the risk of the project title '{project_title}'""",
    expected_output="A categorized project title labeled as 'Technical' or 'Fiction'.",
    agent=risk_analysis_agent, 
)

return_on_investment_task = Task(
    description="""Analyze the return on investment of the project title '{project_title}'""",
    expected_output="A categorized project title labeled as 'Technical' or 'Fiction'.",
    agent=return_on_investment_agent, 
)

final_report_task = Task(
    description="""Review the final responses from the 
                   market demand, risk analysis, and return on investment agents
                   and create a final report.""",
    expected_output="""A comprehensive report on the project title '{project_title}'
    containing the market demand, risk analysis, and return on investment.""",
    agent=manager_agent,
)

from crewai import Crew, Process

project_research_crew = Crew(
    agents=[market_demand_agent, risk_analysis_agent, return_on_investment_agent],
    
    tasks=[market_demand_task, risk_analysis_task, return_on_investment_task, final_report_task],
    
    manager_agent=manager_agent,
    
    process=Process.hierarchical,
    
    verbose=True,
)

inputs = {"project_title": "Amazon INC AMZN"}

result = project_research_crew.kickoff(inputs=inputs)
```

```
────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: a8e117d3-5e21-4071-a992-fbc083e7537f                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
       Status: Executing Task...
    └── 🤖 Agent: Project Research Manager
            Status: In Progress
🤖 Agent: Project Research Manager
    Status: In Progress
🚀 Crew: crew
└── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Market Demand Analyst
            Status: In Progress
🚀 Crew: crew
└── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Market Demand Analyst
            Status: ✅ Completed
🤖 Agent: Market Demand Analyst
    Status: ✅ Completed
└── 🧠 Thinking...
🤖 Agent: Market Demand Analyst
    Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
       Assigned to: Project Research Manager
       Status: ✅ Completed
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: 79cb1712-3a0e-447d-8fdf-2acb64b757ed                                                                     │
│  Agent: Project Research Manager                                                                                │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
       Status: Executing Task...
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
       Status: Executing Task...
    └── 🤖 Agent: Project Research Manager
            Status: In Progress
🤖 Agent: Project Research Manager
    Status: In Progress
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Risk Analysis Analyst
            Status: In Progress
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Risk Analysis Analyst
            Status: ✅ Completed
🤖 Agent: Risk Analysis Analyst
    Status: ✅ Completed
└── 🧠 Thinking...
🤖 Agent: Risk Analysis Analyst
    Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
       Assigned to: Project Research Manager
       Status: ✅ Completed
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: e347e3a3-211f-409e-b706-ed2e5a15cbee                                                                     │
│  Agent: Project Research Manager                                                                                │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
       Status: Executing Task...
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
       Status: Executing Task...
    └── 🤖 Agent: Project Research Manager
            Status: In Progress
🤖 Agent: Project Research Manager
    Status: In Progress
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Return on Investment Analyst
            Status: In Progress
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Return on Investment Analyst
            Status: ✅ Completed
🤖 Agent: Return on Investment Analyst
    Status: ✅ Completed
└── 🧠 Thinking...
🤖 Agent: Return on Investment Analyst
    Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
       Assigned to: Project Research Manager
       Status: ✅ Completed
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c                                                                     │
│  Agent: Project Research Manager                                                                                │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e862ad15-23dd-463b-8c44-1aee15ab7e87
       Status: Executing Task...
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e862ad15-23dd-463b-8c44-1aee15ab7e87
       Status: Executing Task...
    └── 🤖 Agent: Project Research Manager
            Status: In Progress
🤖 Agent: Project Research Manager
    Status: In Progress
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e862ad15-23dd-463b-8c44-1aee15ab7e87
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: In Progress
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e862ad15-23dd-463b-8c44-1aee15ab7e87
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
🤖 Agent: Project Research Manager
    Status: ✅ Completed
└── 🧠 Thinking...
🤖 Agent: Project Research Manager
    Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e862ad15-23dd-463b-8c44-1aee15ab7e87
       Status: Executing Task...
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
🚀 Crew: crew
├── 📋 Task: 79cb1712-3a0e-447d-8fdf-2acb64b757ed
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: e347e3a3-211f-409e-b706-ed2e5a15cbee
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
├── 📋 Task: d4ce7deb-fd9e-4faa-8a2b-686c007d1c7c
│      Assigned to: Project Research Manager
│      Status: ✅ Completed
│   ├── 🤖 Agent: Project Research Manager
│   │       Status: In Progress
│   └── 🤖 Agent: Project Research Manager
│           Status: ✅ Completed
└── 📋 Task: e862ad15-23dd-463b-8c44-1aee15ab7e87
       Assigned to: Project Research Manager
       Status: ✅ Completed
    ├── 🤖 Agent: Project Research Manager
    │       Status: In Progress
    └── 🤖 Agent: Project Research Manager
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: e862ad15-23dd-463b-8c44-1aee15ab7e87                                                                     │
│  Agent: Project Research Manager                                                                                │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: a8e117d3-5e21-4071-a992-fbc083e7537f                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
from IPython.display import Markdown
Markdown(result.raw)
```
