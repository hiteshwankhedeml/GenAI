# 🟢 Sequential Chain

* Previously router was used but now we can use LCEL

```python
prompt1=PromptTemplate(
    template="get a detail report on {topic}",
    input_variables=["topic"]
)

prompt2=PromptTemplate(
    template="generate a 3 point of summary from the following {text}",
    input_variables=["text"]
)

text = "......"

prompt1=PromptTemplate(
    template="analysis the the given text carefully {text} and take the necessary data",
    input_variables=["topic"]
)

prompt2=PromptTemplate(
    template="summarize the given text in 2 bullet points {text}",
    input_variables=["text"]
)

chain = prompt1 | openai_model | parser | prompt2 | openai_model | parser

result=chain.invoke({"text":text})
```
