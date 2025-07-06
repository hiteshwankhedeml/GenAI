# CrewAI - Knowledge - Custom

* &#x20;Chunking in CrewAI is done using tiktoken
*

```python
from crewai.knowledge.source.base_knowledge_source import BaseKnowledgeSource
from typing import Dict, Any
from pydantic import Field
import requests

class WeatherKnowledgeSource(BaseKnowledgeSource):
    """Knowledge source that fetches weather data from an external API."""

    city: str = Field(description="City for which weather should be fetched")

    def load_content(self) -> Dict[Any, str]:
        try:
            print(f"Fetching weather for {self.city}...")

            # Open-Meteo API (no key needed for basic data)
            endpoint = "https://api.open-meteo.com/v1/forecast"
            params = {
                "latitude": 37.77,  # San Francisco by default
                "longitude": -122.42,
                "current_weather": True
            }

            response = requests.get(endpoint, params=params)
            response.raise_for_status()

            weather_data = response.json().get("current_weather", {})
            formatted = self.validate_content(weather_data)
            return {self.city: formatted}

        except Exception as e:
            raise ValueError(f"Failed to fetch weather data: {str(e)}")

    def validate_content(self, data: dict) -> str:
        if not data:
            return "No weather data available."

        return (
            f"Current weather in {self.city}:\n"
            f"- Temperature: {data.get('temperature')}°C\n"
            f"- Wind Speed: {data.get('windspeed')} km/h\n"
            f"- Weather Code: {data.get('weathercode')}\n"
            f"- Time: {data.get('time')}"
        )

    def add(self) -> None:
        """Process and chunk the content."""
        content = self.load_content()
        for _, text in content.items():
            chunks = self._chunk_text(text)
            self.chunks.extend(chunks)
        self._save_documents()
from crewai import Agent, LLM

weather_knowledge = WeatherKnowledgeSource(city="San Francisco")

weather_agent = Agent(
    role="Weather Reporter",
    goal="Answer questions about the current weather forecast.",
    backstory="You are a friendly meteorologist who provides real-time weather updates.",
    knowledge_sources=[weather_knowledge],
    llm=LLM(model="gpt-4o", temperature=0.0),
    verbose=True
)
from crewai import Task, Crew, Process

task = Task(
    description="What is the current temperature and wind speed in San Francisco?",
    expected_output="A concise weather summary for San Francisco.",
    agent=weather_agent
)

crew = Crew(
    agents=[weather_agent],
    tasks=[task],
    process=Process.sequential,
    verbose=True
)
result = crew.kickoff()
print(result)
```
