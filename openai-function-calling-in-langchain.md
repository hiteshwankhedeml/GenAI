---
hidden: true
---

# ✈️ OpenAI Function Calling in LangChain

Pydantic:

* Data validation library
* Works with python type annotations, but rather than static type checking, they are actively used at runtime for data validation and conversion
* Provides built-in methods to serialize/ deserialize models to/from json. dictionaries etc
* Langchain uses pydantic to create json schema describing function
* We define a pydantic class just like a normal class, and list attributes and their types&#x20;

Code:

* &#x20;

```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

from typing import List
from pydantic import BaseModel, Field

class User:
    def __init__(self, name: str, age: int, email: str):
        self.name = name
        self.age = age
        self.email = email

foo = User(name="Joe",age=32, email="joe@gmail.com")

class pUser(BaseModel):
    name: str
    age: int
    email: str

foo_p = pUser(name="Jane", age=32, email="jane@gmail.com")
# Below line will fail
foo_p = pUser(name="Jane", age="bar", email="jane@gmail.com") 

class Class(BaseModel):
    students: List[pUser]

obj = Class(
    students=[pUser(name="Jane", age=32, email="jane@gmail.com")]
)
obj
# Class(students=[pUser(name='Jane', age=32, email='jane@gmail.com')])

class WeatherSearch(BaseModel):
    """Call this with an airport code to get the weather at that airport"""
    airport_code: str = Field(description="airport code to get weather for")

from langchain.utils.openai_functions import convert_pydantic_to_openai_function
weather_function = convert_pydantic_to_openai_function(WeatherSearch)
weather_function
# {'name': 'WeatherSearch',
#  'description': 'Call this with an airport code to get the weather at that airport',
#  'parameters': {'title': 'WeatherSearch',
#   'description': 'Call this with an airport code to get the weather at that airport',
#   'type': 'object',
#   'properties': {'airport_code': {'title': 'Airport Code',
#     'description': 'airport code to get weather for',
#     'type': 'string'}},
#   'required': ['airport_code']}}

class WeatherSearch1(BaseModel):
    airport_code: str = Field(description="airport code to get weather for")
    
convert_pydantic_to_openai_function(WeatherSearch1)

class WeatherSearch2(BaseModel):
    """Call this with an airport code to get the weather at that airport"""
    airport_code: str

convert_pydantic_to_openai_function(WeatherSearch2)
#{'name': 'WeatherSearch2',
# 'description': 'Call this with an airport code to get the weather at that airport',
# 'parameters': {'title': 'WeatherSearch2',
#  'description': 'Call this with an airport code to get the weather at that airport',
#  'type': 'object',
#  'properties': {'airport_code': {'title': 'Airport Code', 'type': 'string'}},
#  'required': ['airport_code']}}

from langchain.chat_models import ChatOpenAI

model = ChatOpenAI()

model.invoke("what is the weather in SF today?", functions=[weather_function])
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})

model_with_function = model.bind(functions=[weather_function])
model_with_function.invoke("what is the weather in sf?")
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})

model_with_forced_function = model.bind(functions=[weather_function], function_call={"name":"WeatherSearch"})
model_with_forced_function.invoke("what is the weather in sf?")
# AIMessage(content='', additional_kwargs={'function_call': {'name':
# 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})

model_with_forced_function.invoke("hi!")
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})

## Using in a chain
from langchain.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant"),
    ("user", "{input}")
])

chain = prompt | model_with_function
chain.invoke({"input": "what is the weather in sf?"})
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})

## Using multiple functions
class ArtistSearch(BaseModel):
    """Call this to get the names of songs by a particular artist"""
    artist_name: str = Field(description="name of artist to look up")
    n: int = Field(description="number of results")

functions = [
    convert_pydantic_to_openai_function(WeatherSearch),
    convert_pydantic_to_openai_function(ArtistSearch),
]

model_with_functions = model.bind(functions=functions)
model_with_functions.invoke("what is the weather in sf?")
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'WeatherSearch', 'arguments': '{"airport_code":"SFO"}'}})

model_with_functions.invoke("what are three songs by taylor swift?")
# AIMessage(content='', additional_kwargs={'function_call': {'name': 'ArtistSearch',
# 'arguments': '{"artist_name":"Taylor Swift","n":3}'}})

model_with_functions.invoke("hi!")



```
