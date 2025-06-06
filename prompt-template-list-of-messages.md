# Prompt Template - List of Messages

```python
from langchain_core.prompts import ChatPromptTemplate

chat_template=ChatPromptTemplate(
    [
        ("system","you are a helpful {domain} expert"),
        ("human","explain the {topic} in simple terms")
    ]
)

prompt = chat_template.invoke({"domain":"medical","topic":"maleria"})
print(prompt)
# messages=[SystemMessage(content='you are a helpful medical expert',
# additional_kwargs={}, response_metadata={}),
# HumanMessage(content='explain the maleria in simple terms',
# additional_kwargs={}, response_metadata={})]

openai_model.invoke(prompt).content

prompt = chat_template.invoke({"domain":"education","topic":"AI"})
```
