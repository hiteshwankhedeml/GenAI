# 🟢 Simple GenAI app using Ollama

* <mark style="color:purple;background-color:purple;">**`ChatPromptTemplate.from_messages`**</mark>
  * <mark style="color:purple;background-color:purple;">**Used when you want to build a prompt from multiple role-based messages (system, user, assistant).**</mark>
  * <mark style="color:purple;background-color:purple;">**Accepts a list of message templates (e.g.,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`SystemMessagePromptTemplate`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`HumanMessagePromptTemplate`**</mark><mark style="color:purple;background-color:purple;">**,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`AIMessagePromptTemplate`**</mark><mark style="color:purple;background-color:purple;">**).**</mark>
  * <mark style="color:purple;background-color:purple;">**Suitable for multi-turn conversations or structured prompts with different roles.**</mark>
* <mark style="color:purple;background-color:purple;">**Steps:**</mark>
  * <mark style="color:purple;background-color:purple;">**Set system prompt message**</mark>
  * <mark style="color:purple;background-color:purple;">**Form chain of llm, prompt, output parser**</mark>
  * <mark style="color:purple;background-color:purple;">**Invoke**</mark>

```python
import os
from dotenv import load_dotenv

from langchain_community.llms import Ollama
import streamlit as st
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.output_parsers import StrOutputParser

load_dotenv()

## Langsmith Tracking
os.environ["LANGCHAIN_API_KEY"]=os.getenv("LANGCHAIN_API_KEY")
os.environ["LANGCHAIN_TRACING_V2"]="true"
os.environ["LANGCHAIN_PROJECT"]=os.getenv("LANGCHAIN_PROJECT")

## Prompt Template
prompt=ChatPromptTemplate.from_messages(
    [
        ("system","You are a helpful assistant. Please respond to the question asked"),
        ("user","Question:{question}")
    ]
)

## streamlit framework
st.title("Langchain Demo With Gemma Model")
input_text=st.text_input("What question you have in mind?")


## Ollama Llama2 model
llm=Ollama(model="gemma:2b")
output_parser=StrOutputParser()
chain=prompt|llm|output_parser

if input_text:
    st.write(chain.invoke({"question":input_text}))



```
