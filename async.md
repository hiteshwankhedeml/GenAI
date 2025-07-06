# Async

* &#x20;

```python
from pydantic import BaseModel

class AIResearchFindings(BaseModel):
    """Represents structured research on AI breakthroughs."""
    title: str
    key_findings: list[str]

class AIRegulationFindings(BaseModel):
    """Represents structured research on AI regulations."""
    region: str
    key_policies: list[str]

class FinalAIReport(BaseModel):
    """Combines AI research & regulation analysis into a report."""
    executive_summary: str
    key_trends: list[str]
    
from crewai import Agent

# Researcher for AI breakthroughs
research_agent = Agent(
    role="AI Researcher",
    goal="Find and summarize the latest AI breakthroughs",
    backstory="An expert AI researcher who tracks technological advancements.",
    verbose=True,
    llm=llm
)

# Analyst for AI regulations
regulation_agent = Agent(
    role="AI Policy Analyst",
    goal="Analyze global AI regulations and summarize policies",
    backstory="A government policy expert specializing in AI ethics and laws.",
    verbose=True,
    llm=llm
)

# Writer for the final AI report
writer_agent = Agent(
    role="AI Report Writer",
    goal="Write a structured report combining AI breakthroughs and regulations",
    backstory="A professional technical writer who crafts AI research reports.",
    verbose=True,
    llm=llm
)

from crewai import Task

# Task 1: AI Breakthroughs Research (Asynchronous)
research_ai_task = Task(
    description="Research the latest AI advancements and summarize key breakthroughs.",
    expected_output="A structured list of AI breakthroughs.",
    agent=research_agent,
    output_pydantic=AIResearchFindings,
    async_execution=True  # Runs asynchronously
)

# Task 2: AI Regulation Analysis (Asynchronous)
research_regulation_task = Task(
    description="Analyze the latest AI regulations worldwide and summarize key policies.",
    expected_output="A structured summary of AI regulations by region.",
    agent=regulation_agent,
    output_pydantic=AIRegulationFindings,
    async_execution=True  # Runs asynchronously
)

# Task 3: Generate AI Research Report (Depends on the first two tasks)
generate_report_task = Task(
    description="Write a structured report summarizing AI breakthroughs and regulations.",
    expected_output="A final AI report summarizing both aspects.",
    agent=writer_agent,
    output_pydantic=FinalAIReport,
    context=[research_ai_task, research_regulation_task]  # Waits for these tasks to complete
)

from crewai import Crew

ai_research_crew = Crew(
    agents=[research_agent, regulation_agent, writer_agent],
    tasks=[research_ai_task, research_regulation_task, generate_report_task],
    verbose=True
)

# Execute the workflow
result = ai_research_crew.kickoff()

# Print the final AI report
print("\n=== Generated AI Report ===")
print(result.raw)

```
