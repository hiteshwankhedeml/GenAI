# 🟢 Json output Parser - 2

* Here we are json list

```python
topic = "............"

template2=PromptTemplate(
    template='Give me 5 facts about {topic} \n {format_instruction}',
    input_variables=['topic'],
    partial_variables={'format_instruction': parser.get_format_instructions()}
)

chain= template2 | openai_model | parser

chain.invoke({"topic": topic})
# {'Facts': [{'Fact 1': '....'},....]}
```
