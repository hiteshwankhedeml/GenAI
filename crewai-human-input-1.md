# CrewAI - Human Input

* &#x20;

```python
import os
from crewai import Agent, Task, Crew, LLM
from crewai_tools import SerperDevTool
from dotenv import load_dotenv
load_dotenv()

llm = LLM(model="gemini/gemini-2.0-flash")

# Loading Tools
search_tool = SerperDevTool()

# Define your agents with roles, goals, tools, and additional attributes
researcher = Agent(
    role='Senior Research Analyst',
    goal='Uncover cutting-edge developments in AI and data science',
    backstory=(
        "You are a Senior Research Analyst at a leading tech think tank. "
        "Your expertise lies in identifying emerging trends and technologies in AI and data science. "
        "You have a knack for dissecting complex data and presenting actionable insights."
    ),
    verbose=True,
    allow_delegation=False,
    tools=[search_tool],
    llm=llm
)
writer = Agent(
    role='Tech Content Strategist',
    goal='Craft compelling content on tech advancements',
    backstory=(
        "You are a renowned Tech Content Strategist, known for your insightful and engaging articles on technology and innovation. "
        "With a deep understanding of the tech industry, you transform complex concepts into compelling narratives."
    ),
    verbose=True,
    allow_delegation=False,
    tools=[search_tool],
    llm=llm
)


# Create tasks for your agents
task1 = Task(
    description=(
        "Conduct a comprehensive analysis of the latest advancements in AI in 2025. "
        "Identify key trends, breakthrough technologies, and potential industry impacts. "
        "Compile your findings in a detailed report. "
        "Make sure to check with a human if the draft is good before finalizing your answer."
    ),
    expected_output='A comprehensive full report on the latest AI advancements in 2025, leave nothing out',
    agent=researcher,
    human_input=True
)

task2 = Task(
    description=(
        "Using the insights from the researcher\'s report, develop an engaging blog post that highlights the most significant AI advancements. "
        "Your post should be informative yet accessible, catering to a tech-savvy audience. "
        "Aim for a narrative that captures the essence of these breakthroughs and their implications for the future."
    ),
    expected_output='A compelling 3 paragraphs blog post formatted as markdown about the latest AI advancements in 2025',
    agent=writer,
    human_input=True
)

# Instantiate your crew with a sequential process
crew = Crew(
    agents=[researcher, writer],
    tasks=[task1, task2],
    verbose=True,

)

# Get your crew to work!
result = crew.kickoff()

print("######################")
print(result)
```
