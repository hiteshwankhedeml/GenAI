# 🟢 Agentic Search Tools

* <mark style="color:purple;background-color:purple;">**If the agent sends a query to search tool, it would first understand the question and divide it into sub questions if needed**</mark>
* <mark style="color:purple;background-color:purple;">**For each subquery, search tool will have to find the best source**</mark>
* <mark style="color:purple;background-color:purple;">**It will get the results, rank and filter the results and result top-k results**</mark>

<mark style="color:purple;background-color:purple;">**Code:**</mark>

* <mark style="color:purple;background-color:purple;">**The result returned is not how human would want it but its exactly how agents needs it**</mark>

```python
### Agentic Search
from dotenv import load_dotenv
import os
from tavily import TavilyClient

# load environment variables from .env file
_ = load_dotenv()

# connect
client = TavilyClient(api_key=os.environ.get("TAVILY_API_KEY"))

# run search
result = client.search("What is in Nvidia's new Blackwell GPU?",
                       include_answer=True)

# print the answer
result["answer"]
# 'The new Blackwell GPU uses the TSMC 4NP node and GDDR7 memory. 
# It features advanced architecture for AI and high-performance computing.
# Blackwell GPUs are designed for next-generation computing tasks.'


### Resgular Search
# choose location (try to change to your own city!)

city = "San Francisco"

query = f"""
    what is the current weather in {city}?
    Should I travel there today?
    "weather.com"
"""

# If we use duckduckgo to get URL
# Then scrape it and pass it to LLM to get results then we might result like
# Recent Locations Weather Forecasts Radar & Maps ......

# If we do using agentic search
result = client.search(query, max_results=1)

# print first result
data = result["results"][0]["content"]

print(data)
# {
#     "location": {
#         "name": "San Francisco",
#         "region": "California",
#         "country": "United States of America",
#         "lat": 37.775,
#         "lon": -122.4183,
#         "tz_id": "America/Los_Angeles",
#         "localtime_epoch": 1757744731,
#         "localtime": "2025-09-12 23:25"
#     },
#     "current": {
#         "last_updated_epoch": 1757744100,
#         "last_updated": "2025-09-12 23:15",
#         "temp_c": 16.7,
#         "temp_f": 62.1,
#         "is_day": 0,
#        "condition": {
#             "text": "Overcast",
#             "icon": "//cdn.weatherapi.com/weather/64x64/night/122.png",
#             "code": 1009
#         },
#         "wind_mph": 8.1,
#         "wind_kph": 13.0,
#         "wind_degree": 250,
#         "wind_dir": "WSW",
#         "pressure_mb": 1015.0,

```
