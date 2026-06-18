# Tools

* We need to use @tool to use a function as a tool
* Models can request to call tools that perform tasks such as fetching data from a database, searching the web, or running code. Tools are pairings of:

1. A schema, including the name of the tool, a description, and/or argument definitions (often a JSON schema)
2. A function or coroutine to execute.

```python
import os
from langchain.chat_models import init_chat_model
from langchain.tools import tool

model = init_chat_model("groq:qwen/qwen3-32b")
response = model.invoke("Why do parrots talk?")



@tool
def get_weather(location:str)->str:
    """Get the weather at a location"""
    return f"It's sunny in {location}"


model_with_tools=model.bind_tools([get_weather])

response = model_with_tools.invoke("What's the weather like in Boston?")
print(response)
for tool_call in response.tool_calls:
    # View tool calls made by the model
    print(f"Tool: {tool_call['name']}")
    print(f"Args: {tool_call['args']}")
# content='' additional_kwargs={'reasoning_content': "Okay, the user is asking about 
# the weather in Boston. I need to use the get_weather function. Let me check the 
## function parameters. It requires a location, which is Boston here. I'll call the
##  function with location set to Boston. Make sure the JSON is correctly formatted 
## with the name and arguments.\n", 'tool_calls': [{'id': 'c6smtcmmb', 'function': 
## {'arguments': '{"location":"Boston"}', 'name': 'get_weather'}, 'type': 'function'}]}
## response_metadata={'token_usage': {'completion_tokens': 86, 'prompt_tokens': 154,
##  'total_tokens': 240, 'completion_time': 0.160528624, 'completion_tokens_details':
## {'reasoning_tokens': 62}, 'prompt_time': 0.005997383, 'prompt_tokens_details': None,
## 'queue_time': 0.057710677, 'total_time': 0.166526007}, 'model_name': 'qwen/qwen3-32b',
## 'system_fingerprint': 'fp_5cf921caa2', 'service_tier': 'on_demand', 'finish_reason':
## 'tool_calls', 'logprobs': None, 'model_provider': 'groq'}
## id='lc_run--e7bb7c48-663e-47ad-bb73-fdc081fa0fcc-0' tool_calls=[{'name': 'get_weather',
## 'args': {'location': 'Boston'}, 'id': 'c6smtcmmb', 'type': 'tool_call'}]
## usage_metadata={'input_tokens': 154, 'output_tokens': 86, 'total_tokens': 240,
## 'output_token_details': {'reasoning': 62}}
# Tool: get_weather
# Args: {'location': 'Boston'}
```

