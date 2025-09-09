---
hidden: true
---

# ✈️ Implementation

How to access the huggingface API:

* There are also two ways to use this class. You can specify the model with the repo\_id parameter.
* Those endpoints use the serverless API, which is particularly beneficial to people using pro accounts or enterprise hub.
* Still, regular users can already have access to a fair amount of request by connecting with their HF token in the environment where they are executing the code

```python
from langchain_huggingface import HuggingFaceEndpoint

repo_id="mistralai/Mistral-7B-Instruct-v0.3"
llm=HuggingFaceEndpoint(repo_id=repo_id,max_length=150,temperature=0.7,token=os.getenv("HF_TOKEN"))
llm

llm.invoke("What is machine learning")

repo_id="google/gemma-2-9b"
llm=HuggingFaceEndpoint(repo_id=repo_id,max_length=150,temperature=0.7,token=os.getenv("HF_TOKEN"))
# This wont work, as size is too big, so we need to have a dedicated endpoint
llm.invoke("What is machine learning")

from langchain import PromptTemplate,LLMChain
template="""
Question:{question}
Answer:Lets think step by step.
"""
prompt=PromptTemplate(template=template,input_variables=["question"])
print(prompt)

llm_chain=LLMChain(llm=llm,prompt=prompt)
llm.invoke("Who won the cricket World up 2011")

from langchain_community.embeddings import HuggingFaceBgeEmbeddings

model_name = "BAAI/bge-small-en"
model_kwargs = {"device": "cpu"}
encode_kwargs = {"normalize_embeddings": True}
hf = HuggingFaceBgeEmbeddings(
    model_name=model_name, model_kwargs=model_kwargs, encode_kwargs=encode_kwargs
)

embedding = hf.embed_query("hi this is harrison")

```
