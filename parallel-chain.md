# 🟢 Parallel Chain

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**We create a parallel chain**</mark>
* <mark style="color:purple;background-color:purple;">**It will be having key value form**</mark>
* <mark style="color:purple;background-color:purple;">**Key will be output of the chain, value will have the chain to be run**</mark>
* <mark style="color:purple;background-color:purple;">**We can even pass the output of the parallel to chain to another chain**</mark>
* <mark style="color:purple;background-color:purple;">**The keys of the parallel chain will be input to the chain**</mark>

```python
prompt1=PromptTemplate(
    template="generate simple summary from the following text \n {text}",
    input_variables=["text"]
)

prompt2=PromptTemplate(
    template="generate 3 question and answer from the following text \n {text}",
    input_variables=["text"]
    )

prompt3=PromptTemplate(
    template="analysis the summary and qa and generate the 5 important quiz with 4 possible answer \n summary: {summary}, Q&A: {qa}",
    input_variables=["summary","qa"]
)

from langchain.schema.runnable import RunnableParallel

parallel_chain=RunnableParallel({
    "summary": prompt1 | openai_model | parser,
    "qa" : prompt2 | openai_model | parser
}
)

parallel_chain.invoke({"text":text})
#{
#"summary":"......",
#"qa":"......."
#}

merge_chain= prompt3 | openai_model | parser
chain = parallel_chain | merge_chain
chain.get_graph().print_ascii()
```
