# Pydantic - GuardRails

* &#x20;

````python
from pydantic import BaseModel

class ResearchReport(BaseModel):
    """Represents a structured research report"""
    title: str
    summary: str
    key_findings: list[str]
import json
from typing import Tuple, Any

def validate_json_report(result: str) -> Tuple[bool, Any]:
    """Ensures AI-generated output is valid JSON with required fields."""
    try:
        # Parse JSON output
        data = json.loads(result.pydantic.model_dump_json())

        # Check required fields
        if "title" not in data or "summary" not in data or "key_findings" not in data:
            return (False, "Missing required fields: title, summary, or key_findings.")

        return (True, result)  # JSON is valid
    except json.JSONDecodeError:
        return (False, "Invalid JSON format. Please ensure correct syntax.")
from crewai import Agent

# Create the AI Agent
research_report_agent = Agent(
    role="Research Analyst",
    goal="Generate structured JSON reports for research papers",
    backstory="You are an expert in technical writing and structured reporting.",
    verbose=False,
    llm=llm
)
from crewai import Task

research_report_task = Task(
    description="Generate a structured research report in valid JSON format.",
    expected_output="A valid JSON object containing 'title', 'summary', and 'key_findings'.",
    agent=research_report_agent,
    output_pydantic=ResearchReport,  # Ensures structured output
    guardrail=validate_json_report,  # Validate output before passing to next step
    max_retries=3  # Allow up to 3 retries if validation fails
)
from crewai import Crew

research_crew = Crew(
    agents=[research_report_agent],
    tasks=[research_report_task],
    verbose=True  # Display execution details
)
result = research_crew.kickoff()

# Display the validated JSON output
print("Final Research Report:", result.pydantic)
╭──────────────────────────────────────────── Crew Execution Started ─────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Started                                                                                         │
│  Name: crew                                                                                                     │
│  ID: 476d06be-9c0d-4be6-8c30-07adf3fa725e                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
🚀 Crew: crew
└── 📋 Task: ef4d2f1a-fa57-4062-bc18-3068e2136064
       Status: Executing Task...
🚀 Crew: crew
└── 📋 Task: ef4d2f1a-fa57-4062-bc18-3068e2136064
       Status: Executing Task...
    └── 🤖 Agent: Research Analyst
            Status: In Progress
# Agent: Research Analyst
## Task: Generate a structured research report in valid JSON format.


# Agent: Research Analyst
## Final Answer: 
```json
{
  "title": "The Impact of Artificial Intelligence on Healthcare",
  "summary": "This research investigates the transformative impact of Artificial Intelligence (AI) on the healthcare industry. It explores various applications of AI, including diagnostics, treatment planning, drug discovery, and patient monitoring. The study analyzes the benefits, challenges, and ethical considerations associated with AI adoption in healthcare, providing insights into its potential to improve patient outcomes, reduce costs, and enhance the overall efficiency of healthcare systems.",
  "key_findings": [
    "AI-powered diagnostic tools demonstrate improved accuracy and speed in detecting diseases compared to traditional methods.",
    "AI algorithms can personalize treatment plans based on individual patient characteristics, leading to better outcomes.",
    "AI accelerates drug discovery by identifying potential drug candidates and predicting their efficacy.",
    "AI-enabled remote patient monitoring systems improve patient engagement and reduce hospital readmissions.",
    "Ethical concerns regarding data privacy, algorithmic bias, and the potential displacement of healthcare professionals need careful consideration."
  ]
}
```


🚀 Crew: crew
└── 📋 Task: ef4d2f1a-fa57-4062-bc18-3068e2136064
       Status: Executing Task...
    └── 🤖 Agent: Research Analyst
            Status: ✅ Completed
🚀 Crew: crew
└── 📋 Task: ef4d2f1a-fa57-4062-bc18-3068e2136064
       Assigned to: Research Analyst
       Status: ✅ Completed
    └── 🤖 Agent: Research Analyst
            Status: ✅ Completed
╭──────────────────────────────────────────────── Task Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Task Completed                                                                                                 │
│  Name: ef4d2f1a-fa57-4062-bc18-3068e2136064                                                                     │
│  Agent: Research Analyst                                                                                        │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭──────────────────────────────────────────────── Crew Completion ────────────────────────────────────────────────╮
│                                                                                                                 │
│  Crew Execution Completed                                                                                       │
│  Name: crew                                                                                                     │
│  ID: 476d06be-9c0d-4be6-8c30-07adf3fa725e                                                                       │
│                                                                                                                 │
│                                                                                                                 │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
Final Research Report: title='The Impact of Artificial Intelligence on Healthcare' summary='This research investigates the transformative impact of Artificial Intelligence (AI) on the healthcare industry. It explores various applications of AI, including diagnostics, treatment planning, drug discovery, and patient monitoring. The study analyzes the benefits, challenges, and ethical considerations associated with AI adoption in healthcare, providing insights into its potential to improve patient outcomes, reduce costs, and enhance the overall efficiency of healthcare systems.' key_findings=['AI-powered diagnostic tools demonstrate improved accuracy and speed in detecting diseases compared to traditional methods.', 'AI algorithms can personalize treatment plans based on individual patient characteristics, leading to better outcomes.', 'AI accelerates drug discovery by identifying potential drug candidates and predicting their efficacy.', 'AI-enabled remote patient monitoring systems improve patient engagement and reduce hospital readmissions.', 'Ethical concerns regarding data privacy, algorithmic bias, and the potential displacement of healthcare professionals need careful consideration.']
````
