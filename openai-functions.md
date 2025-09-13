# OpenAI Functions

* Here we have hardcoded function current\_weather ⇒ in actual we will be calling API inside this
* To define a function we need
  * name
  * description
  * parameters
  * Required parameters
* Description are very important as it will be passed to LLM
* Initially there will be nothing in response, as it will not generate as it does not generates and just gives the name of function which is to be called
* if the message passed is not related to weather, then in response there will be content and there won't be any function calling
* By default function\_call is auto, this means that language model chooses
* If we pass function\_call as none, this means that none of the function will be used
* If we pass name of the function\_call then it will force the function to be called
* Function calls takes up tokens
* We pass function call in the message and the to the llm
* llm will take the output of function and will convert it into nice message

```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

import json

# Example dummy function hard coded to return the same weather
# In production, this could be your backend API or an external API
def get_current_weather(location, unit="fahrenheit"):
    """Get the current weather in a given location"""
    weather_info = {
        "location": location,
        "temperature": "72",
        "unit": unit,
        "forecast": ["sunny", "windy"],
    }
    return json.dumps(weather_info)
    
# define a function
functions = [
    {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA",
                },
                "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]},
            },
            "required": ["location"],
        },
    }
]

messages = [
    {
        "role": "user",
        "content": "What's the weather like in Boston?"
    }
]

import openai

# Call the ChatCompletion endpoint
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions
)

print(response)
#{
#  "id": "chatcmpl-AVCsmqFCt4FILgYpy7wDxpjahOZvC",
#  "object": "chat.completion",
#  "created": 1732001052,
#  "model": "gpt-3.5-turbo-0125",
#  "choices": [
#    {
#      "index": 0,
#      "message": {
#        "role": "assistant",
#        "content": null,
#        "function_call": {
#          "name": "get_current_weather",
#          "arguments": "{\"location\":\"Boston\",\"unit\":\"celsius\"}"
#        },
#        "refusal": null
#      },
#      "logprobs": null,
#      "finish_reason": "function_call"
#    }
#  ],
#  "usage": {
#    "prompt_tokens": 82,
#    "completion_tokens": 20,
#    "total_tokens": 102,
#    "prompt_tokens_details": {
#      "cached_tokens": 0,
#      "audio_tokens": 0
#    },
#    "completion_tokens_details": {
#      "reasoning_tokens": 0,
#      "audio_tokens": 0,
#      "accepted_prediction_tokens": 0,
#      "rejected_prediction_tokens": 0
#    }
#  },
#  "system_fingerprint": null
#}

response_message = response["choices"][0]["message"]

json.loads(response_message["function_call"]["arguments"])
# {'location': 'Boston', 'unit': 'celsius'}

args = json.loads(response_message["function_call"]["arguments"])
get_current_weather(args)
# '{"location": {"location": "Boston", "unit": "celsius"},
# "temperature": "72", "unit": "fahrenheit",
# "forecast": ["sunny", "windy"]}'

messages = [
    {
        "role": "user",
        "content": "What's the weather in Boston?",
    }
]
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="none",
)
print(response)

messages = [
    {
        "role": "user",
        "content": "What's the weather like in Boston!",
    }
]
response = openai.ChatCompletion.create(
    # OpenAI Updates: As of June 2024, we are now using the GPT-3.5-Turbo model
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call={"name": "get_current_weather"},
)
print(response)

args = json.loads(response["choices"][0]["message"]['function_call']['arguments'])
observation = get_current_weather(args)

messages.append(
        {
            "role": "function",
            "name": "get_current_weather",
            "content": observation,
        }
)

response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)
print(response)
#{
#  "id": "chatcmpl-BFWKVL8edWMlnpwxIT0gAvcmOrNyq",
#  "object": "chat.completion",
#  "created": 1743038895,
#  "model": "gpt-3.5-turbo-0125",
#  "choices": [
#    {
#      "index": 0,
#      "message": {
#        "role": "assistant",
#        "content": "The current weather in Boston is sunny and windy with a temperature of 72\u00b0F (22\u00b0C).",
#        "refusal": null,
#        "annotations": []
#      },
#      "logprobs": null,
#      "finish_reason": "stop"
#    }
#  ],
#  "usage": {
#    "prompt_tokens": 84,
#    "completion_tokens": 21,
#    "total_tokens": 105,
#    "prompt_tokens_details": {
#      "cached_tokens": 0,
#      "audio_tokens": 0
#    },
#    "completion_tokens_details": {
#      "reasoning_tokens": 0,
#      "audio_tokens": 0,
#      "accepted_prediction_tokens": 0,
#      "rejected_prediction_tokens": 0
#    }
#  },
#  "service_tier": "default",
#  "system_fingerprint": null
#}

```
