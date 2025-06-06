# Prompt Template

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
