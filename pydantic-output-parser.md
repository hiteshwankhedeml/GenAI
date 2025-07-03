# 🟢 Pydantic Output parser

* For Validation
* <mark style="color:purple;background-color:purple;">**We want to apply more structured output with conditions**</mark>
* <mark style="color:purple;background-color:purple;">**we can also apply data types for each field, like int for age**</mark>

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Create a schema using BaseModel of pydantic**</mark>
* <mark style="color:purple;background-color:purple;">**Define a pydantic parser using the above schema**</mark>
* <mark style="color:purple;background-color:purple;">**Pass this parser in prompt template in format instructions**</mark>

```python
from pydantic import BaseModel, Field
from langchain_core.output_parsers import PydanticOutputParser

class Person(BaseModel):
    name:str=Field(description="name of person")
    age:int=Field(gt=18,description="age of person")
    city:str=Field(description="name of the city where the person is located")
    
parser=PydanticOutputParser(pydantic_object=Person)

parser.get_format_instructions()

template = PromptTemplate(
    template='Generate the nammme, age and city of a fictional {place} person \n {format_instruction}',
    input_variables=['place'],
    partial_variables={'format_instruction':parser.get_format_instructions()}
)

chain = template | openai_model | parser

result=chain.invoke({"place":"bengaluru"})

result
# Person(name='Rohit Sharma', age=29, city='Bengaluru')
```
