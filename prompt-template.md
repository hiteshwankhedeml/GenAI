# 🟢 Prompt Template

* <mark style="color:purple;background-color:purple;">**We define template with {placeholders} and also specify this placeholders in input\_varoables**</mark>
* <mark style="color:purple;background-color:purple;">**When invoking, we pass the values of this placeholders**</mark>

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Define the template**</mark>
* <mark style="color:purple;background-color:purple;">**Invoke the template and pass the values of the placeholders**</mark>
* <mark style="color:purple;background-color:purple;">**llm.invoke(prompt)**</mark>

```python
from langchain_core.prompts import PromptTemplate

template=PromptTemplate(
    template="can you say hello to {name} in 5 different language",
    input_variables=['name']
)

template
# PromptTemplate(input_variables=['name'], input_types={}, partial_variables={},
# template='can you say hello to {name} in 5 different language')

template.get_prompts()
# [PromptTemplate(input_variables=['name'], input_types={}, partial_variables={},
# template='can you say hello to {name} in 5 different language')]

template.invoke({"name":"sunny"})
# StringPromptValue(text='can you say hello to sunny in 5 different language')

openai_model.invoke(prompt).content


```
