# 🟢 Json output Parser

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Here in prompt template we are giving format instructions for JsonOutput Parser**</mark>
* <mark style="color:purple;background-color:purple;">**When we run the chain, we get json output**</mark>

```python
from langchain_core.output_parsers import JsonOutputParser

parser=JsonOutputParser()

parser.get_format_instructions()
# 'Return a JSON object.'

template=PromptTemplate(
    template="give me name, age and city from the provided text {text} \n {format_instructions}" ,
    input_variables=['text'],
    partial_variables={"format_instructions":parser.get_format_instructions()}
)

text="""hi my name is sunny savita my age is 29 and i am belong to bengaluru"""

prompt=template.format(text=text)

result=openai_model.invoke(prompt)

result.content
# '{\n"name": "sunny savita",\n"age": 29,\n"city": "bengaluru"\n}'

chain= template | openai_model | parser

chain.invoke({"text":text})
```
