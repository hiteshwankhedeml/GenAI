# 🟢 Prompt

* <mark style="color:purple;background-color:purple;">**System Message - For the behaviour of the LLM**</mark>
* <mark style="color:purple;background-color:purple;">**Human Message -**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**AI Message -**</mark>&#x20;

```python
from langchain_core.messages import SystemMessage,HumanMessage,AIMessage

My_Model:"GPT"
System_Message: "You are healthcare chatbot."
Human_Message or User_Message: "can you suggest me a best medicine for fever?"
AI_Message or Model_Generated_Message: "paracetamol/DOLO650 is a best medicine for fever."

SystemMessage(content="you are a funny bot means whatever you answer, you answer in the funny way")
# SystemMessage(content='you are a funny bot means whatever you answer,
# you answer in the funny way', additional_kwargs={}, response_metadata={}

HumanMessage(content="who is your best friend")
# HumanMessage(content='who is your best friend',
# additional_kwargs={}, response_metadata={})

messages=[SystemMessage(content="you are a funny bot means whatever you answer, you answer in the funny way"),
          HumanMessage(content="who is your best friend")]
          
messages2=[SystemMessage(content="you are angery young man, you answer everything in rude way"),
          HumanMessage(content="who is your best friend")]

openai_model.invoke(messages).content
openai_model.invoke(messages2).content

messages3=[SystemMessage(content="you are very helpful assistance you answer everything in detail"),
          HumanMessage(content="tell me the role of langchain in AI devlopment")]
messages3.append(AIMessage(openai_model.invoke(messages3).content))
messages3
# [SystemMessage(content='you are very helpful.........
# HumanMessage(content='tell me the role of la.........
# AIMessage(content='Langchain is a term that .........

```
