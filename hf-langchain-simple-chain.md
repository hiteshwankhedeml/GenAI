# 🟢 HF - LangChain - Simple Chain

* <mark style="color:purple;background-color:purple;">**We need to initiate huggingface pipeline**</mark>
* <mark style="color:purple;background-color:purple;">**We can pass this pipeline to langchain-huggingface interface**</mark>
* <mark style="color:purple;background-color:purple;">**After this we can use all the langchain components**</mark>

```python
from langchain_huggingface import HuggingFacePipeline
from transformers import AutoModelForCausalLM, AutoTokenizer, pipeline

model_id="gpt2"
model=AutoModelForCausalLM.from_pretrained(model_id)
tokenizer=AutoTokenizer.from_pretrained(model_id)


pipe=pipeline("text-generation",model=model,tokenizer=tokenizer,max_new_tokens=100)

hf=HuggingFacePipeline(pipeline=pipe)

hf.invoke("hi hello how are you?")

##---------Using prompt and chain--------------
template="""Quiestion: {queston}
give me a answer in detail manner and in step by step manner"""

prompt=PromptTemplate(template=template,input_variables=["question"])

llm_chain.invoke(question)
```
