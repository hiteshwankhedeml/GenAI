# CrewAI - Callback

* &#x20;

```python
from crewai import Agent

# AI Researcher Agent
research_agent = Agent(
    role="AI News Researcher",
    goal="Find and summarize the latest AI news from trusted sources",
    backstory="You are a dedicated AI journalist who follows the latest advancements in artificial intelligence.",
    verbose=False,
    llm=llm
)

def notify_team(output):

    print(f"""Task Completed!
              Task: {output.description}
              Output Summary: {output.summary}""")

    with open("latest_ai_news.txt", "w") as f:
        f.write(f"Task: {output.description}\n")
        f.write(f"Output Summary: {output.summary}\n")
        f.write(f"Full Output: {output.raw}\n")
    
    print("News summary saved to latest_ai_news.txt")

from crewai import Task

research_news_task = Task(
    description="Find and summarize the latest AI breakthroughs from the last week.",
    expected_output="A structured summary of AI news headlines.",
    agent=research_agent,
    callback=notify_team  # Calls the function after task completion
)

from crewai import Crew

ai_news_crew = Crew(
    agents=[research_agent],
    tasks=[research_news_task],
    verbose=False
)

# Execute the workflow
result = ai_news_crew.kickoff()
```
