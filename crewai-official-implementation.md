# CrewAI - Official Implementation

**Steps:**

* This will create the project structure
  * crewai create crew <\<project\_name>>    \
    cd <\<project\_name>>
* It will ask llm provide, then model, API
* Modify Agent.yaml
* Modify tasks.yaml
* Modify crew.py
* Setup main scripy.py
* Setup env variables
* crewai run

Project Structure:

```python
research_crew/
├── .gitignore
├── pyproject.toml
├── README.md
├── .env
└── src/
    └── research_crew/
        ├── __init__.py
        ├── main.py
        ├── crew.py
        ├── tools/
        │   ├── custom_tool.py
        │   └── __init__.py
        └── config/
            ├── agents.yaml
            └── tasks.yaml
```

Agent.yaml

```python
# src/research_crew/config/agents.yaml
researcher:
  role: >
    Senior Research Specialist for {topic}
  goal: >
    Find comprehensive and accurate information about {topic}
    with a focus on recent developments and key insights
  backstory: >
    You are an experienced research specialist with a talent for
    finding relevant information from various sources. You excel at
    organizing information in a clear and structured manner, making
    complex topics accessible to others.
  llm: provider/model-id  # e.g. openai/gpt-4o, google/gemini-2.0-flash, anthropic/claude...

analyst:
  role: >
    Data Analyst and Report Writer for {topic}
  goal: >
    Analyze research findings and create a comprehensive, well-structured
    report that presents insights in a clear and engaging way
  backstory: >
    You are a skilled analyst with a background in data interpretation
    and technical writing. You have a talent for identifying patterns
    and extracting meaningful insights from research data, then
    communicating those insights effectively through well-crafted reports.
  llm: provider/model-id  # e.g. openai/gpt-4o, google/gemini-2.0-flash, anthropic/claude...
```

Tasks.yaml

```python
# src/research_crew/config/tasks.yaml
research_task:
  description: >
    Conduct thorough research on {topic}. Focus on:
    1. Key concepts and definitions
    2. Historical development and recent trends
    3. Major challenges and opportunities
    4. Notable applications or case studies
    5. Future outlook and potential developments

    Make sure to organize your findings in a structured format with clear sections.
  expected_output: >
    A comprehensive research document with well-organized sections covering
    all the requested aspects of {topic}. Include specific facts, figures,
    and examples where relevant.
  agent: researcher

analysis_task:
  description: >
    Analyze the research findings and create a comprehensive report on {topic}.
    Your report should:
    1. Begin with an executive summary
    2. Include all key information from the research
    3. Provide insightful analysis of trends and patterns
    4. Offer recommendations or future considerations
    5. Be formatted in a professional, easy-to-read style with clear headings
  expected_output: >
    A polished, professional report on {topic} that presents the research
    findings with added analysis and insights. The report should be well-structured
    with an executive summary, main sections, and conclusion.
  agent: analyst
  context:
    - research_task
  output_file: output/report.md
```

crew.py

```python
# src/research_crew/crew.py
from crewai import Agent, Crew, Process, Task
from crewai.project import CrewBase, agent, crew, task
from crewai_tools import SerperDevTool
from crewai.agents.agent_builder.base_agent import BaseAgent
from typing import List

@CrewBase
class ResearchCrew():
    """Research crew for comprehensive topic analysis and reporting"""

    agents: List[BaseAgent]
    tasks: List[Task]

    @agent
    def researcher(self) -> Agent:
        return Agent(
            config=self.agents_config['researcher'], # type: ignore[index]
            verbose=True,
            tools=[SerperDevTool()]
        )

    @agent
    def analyst(self) -> Agent:
        return Agent(
            config=self.agents_config['analyst'], # type: ignore[index]
            verbose=True
        )

    @task
    def research_task(self) -> Task:
        return Task(
            config=self.tasks_config['research_task'] # type: ignore[index]
        )

    @task
    def analysis_task(self) -> Task:
        return Task(
            config=self.tasks_config['analysis_task'], # type: ignore[index]
            output_file='output/report.md'
        )

    @crew
    def crew(self) -> Crew:
        """Creates the research crew"""
        return Crew(
            agents=self.agents,
            tasks=self.tasks,
            process=Process.sequential,
            verbose=True,
        )
```

main.py

```python
#!/usr/bin/env python
# src/research_crew/main.py
import os
from research_crew.crew import ResearchCrew

# Create output directory if it doesn't exist
os.makedirs('output', exist_ok=True)

def run():
    """
    Run the research crew.
    """
    inputs = {
        'topic': 'Artificial Intelligence in Healthcare'
    }

    # Create and run the crew
    result = ResearchCrew().crew().kickoff(inputs=inputs)

    # Print the result
    print("\n\n=== FINAL REPORT ===\n\n")
    print(result.raw)

    print("\n\nReport has been saved to output/report.md")

if __name__ == "__main__":
    run()
```
