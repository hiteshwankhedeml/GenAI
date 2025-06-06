# Pydantic Output parser

* For Validation
* We want to apply more structured output with conditions
* we can also apply data types for each field, like int for age

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
