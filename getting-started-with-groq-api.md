# 🟢 Getting started with Groq API

* <mark style="color:purple;background-color:purple;">**Nothing much here, just we have used groq in this exercise**</mark>
* LCEL is used to chain components together
* [https://groq.com/about-us/](https://groq.com/about-us/) ⇒ To access open source models
* They have used LPU inferencing engine
* Why Groq?
  * <mark style="color:purple;background-color:purple;">**Deliver fast AI inference**</mark>
  * <mark style="color:purple;background-color:purple;">**LPU is hardware and software platform, which is very fast**</mark>

```python
### Open AI API Key and Open Source models--Llama3,Gemma2,mistral--Groq
import os
from dotenv import load_dotenv
load_dotenv()

import openai
openai.api_key=os.getenv("OPENAI_API_KEY")

groq_api_key=os.getenv("GROQ_API_KEY")
groq_api_key

from langchain_openai import ChatOpenAI
from langchain_groq import ChatGroq
model=ChatGroq(model="Gemma2-9b-It",groq_api_key=groq_api_key)

from langchain_core.messages import HumanMessage,SystemMessage

messages=[
    SystemMessage(content="Translate the following from English to French"),
    HumanMessage(content="Hello How are you?")
]

result=model.invoke(messages)

from langchain_core.output_parsers import StrOutputParser
parser=StrOutputParser()
parser.invoke(result)

### Using LCEL- chain the components
chain=model|parser
chain.invoke(messages)

### Prompt Templates
from langchain_core.prompts import ChatPromptTemplate

generic_template="Trnaslate the following into {language}:"

prompt=ChatPromptTemplate.from_messages(
    [("system",generic_template),("user","{text}")]
)

result=prompt.invoke({"language":"French","text":"Hello
result.to_messages()
#[SystemMessage(content='Trnaslate the following into French:'),
# HumanMessage(content='Hello')]

##Chaining together components with LCEL
chain=prompt|model|parser
chain.invoke({"language":"French","text":"Hello"})




```
