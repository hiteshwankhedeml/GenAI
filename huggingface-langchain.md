# 🟢 HuggingFace - LangChain

* <mark style="color:purple;background-color:purple;">**We can also use huggingface models using langchain huggingfacehub, while loading model we need to specify huggingface repo id etc**</mark>
* Access huggingface using langchain
* In the below example, inference API is getting use
* We also using simple chain here

```python
!pip install langchain-huggingface
!pip install transformers
!pip install accelerate
!pip install  bitsandbytes
!pip install langchain_community
!pip install --upgrade transformers accelerate bitsandbytes
!pip install huggingface_hub

from google.colab import userdata
import os

hf_token=userdata.get('HF_TOKEN')
os.environ["HUGGINGFACEHUB_API_TOKEN"]=hf_token

repo_id="deepseek-ai/DeepSeek-R1"

from langchain import HuggingFaceHub
llm=HuggingFaceHub(repo_id=repo_id,model_kwargs={"temperature":0.1,"max_length":64})

from langchain import PromptTemplate, LLMChain

question="who is a first president of INDIA?"

template="""Quiestion: {queston}
give me a answer in detail manner and in step by step manner"""

prompt=PromptTemplate(template=template,input_variables=["question"])

prompt
# PromptTemplate(input_variables=['queston'], input_types={},
# partial_variables={}, template='Quiestion: {queston}\ngive me a 
# answer in detail manner and in step by step manner')

llm.invoke("hello how are you?")

llm_chain=LLMChain(llm=llm,prompt=prompt)
llm_chain.invoke(question)

```
