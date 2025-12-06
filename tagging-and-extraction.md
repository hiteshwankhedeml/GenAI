---
hidden: true
---

# ✈️ Tagging and Extraction

* Tagging helps us to extract structured data from unstructured data
* Extraction helps us to extract specific entities from the text

```python
import os
import openai

from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']

from typing import List
from pydantic import BaseModel, Field
from langchain.utils.openai_functions import convert_pydantic_to_openai_function

class Tagging(BaseModel):
    """Tag the piece of text with particular info."""
    sentiment: str = Field(description="sentiment of text, should be `pos`, `neg`, or `neutral`")
    language: str = Field(description="language of text (should be ISO 639-1 code)")

convert_pydantic_to_openai_function(Tagging)

from langchain.prompts import ChatPromptTemplate
from langchain.chat_models import ChatOpenAI

model = ChatOpenAI(temperature=0)

tagging_functions = [convert_pydantic_to_openai_function(Tagging)]

prompt = ChatPromptTemplate.from_messages([
    ("system", "Think carefully, and then tag the text as instructed"),
    ("user", "{input}")
])

model_with_functions = model.bind(
    functions=tagging_functions,
    function_call={"name": "Tagging"}
)

tagging_chain = prompt | model_with_functions
tagging_chain.invoke({"input": "I love langchain"})
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'Tagging', 'arguments': '{"sentiment":"pos","language":"en"}'}}

tagging_chain.invoke({"input": "non mi piace questo cibo"})

from langchain.output_parsers.openai_functions import JsonOutputFunctionsParser

tagging_chain = prompt | model_with_functions | JsonOutputFunctionsParser()
tagging_chain.invoke({"input": "non mi piace questo cibo"})
# {'sentiment': 'neg', 'language': 'it'}

## Extraction
from typing import Optional
class Person(BaseModel):
    """Information about a person."""
    name: str = Field(description="person's name")
    age: Optional[int] = Field(description="person's age")

class Information(BaseModel):
    """Information to extract."""
    people: List[Person] = Field(description="List of info about people")

convert_pydantic_to_openai_function(Information)
# {'name': 'Information',
#  'description': 'Information to extract.',
#  'parameters': {'title': 'Information',
#   'description': 'Information to extract.',
#   'type': 'object',
#   'properties': {'people': {'title': 'People',
#     'description': 'List of info about people',
#     'type': 'array',
#     'items': {'title': 'Person',
#      'description': 'Information about a person.',
#      'type': 'object',
#      'properties': {'name': {'title': 'Name',
#        'description': "person's name",
#        'type': 'string'},
#       'age': {'title': 'Age',
#        'description': "person's age",
#        'type': 'integer'}},
#      'required': ['name']}}},
#   'required': ['people']}}

extraction_functions = [convert_pydantic_to_openai_function(Information)]
extraction_model = model.bind(functions=extraction_functions, function_call={"name": "Information"})

extraction_model.invoke("Joe is 30, his mom is Martha")
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'Information', 'arguments': '{"people":[{"name":"Joe",
# "age":30},{"name":"Martha"}]}'}})

prompt = ChatPromptTemplate.from_messages([
    ("system", "Extract the relevant information, if not explicitly provided do not guess. Extract partial info"),
    ("human", "{input}")
])

extraction_chain = prompt | extraction_model
# AIMessage(content='', additional_kwargs={'function_call':
# {'name': 'Information', 'arguments': '{"people":[{"name":"Joe","age":30},
# {"name":"Martha"}]}'}})

extraction_chain = prompt | extraction_model | JsonOutputFunctionsParser()

```
