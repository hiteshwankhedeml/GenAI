# 🟢 Introduction to Text Summarization with LangChain

*

```
<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
```

* Data source can be structured / unstructued
* Files will be read and then summarization will be done
* <mark style="color:purple;background-color:purple;">3 approaches for summarization</mark>
  * <mark style="color:purple;background-color:purple;">Stuff</mark>
  * <mark style="color:purple;background-color:purple;">Map reduce</mark>
  * <mark style="color:purple;background-color:purple;">Refine</mark>
* LLMChain - To combine LLM with prompt
* <mark style="color:purple;background-color:purple;">**If text is less then we can do using prompt, for large text we need to use text summarization technique**</mark>

```python
from langchain_groq import ChatGroq
api_key=os.getenv("GROQ_API_KEY")
llm=ChatGroq(groq_api_key=api_key,model="Gemma-7b-It")
llm

from langchain.schema import(
    AIMessage,
    HumanMessage,SystemMessage
)

speech="""
People across .....
"""

chat_message=[
    SystemMessage(content="You are expert with experise in summarizing speeched"),
    HumanMessage(content=f"Please provide a short and concisse 
                         summary of the follow speech:\n Text:{speech}")
]

llm.get_num_tokens(speech) # No. of tokens in the speech => 909

## getting the summary - 1st way
llm(chat_message).content # This will also give summary

## We can also create prompt template for text summarization
from langchain.chains import LLMChain
from langchain import PromptTemplate

generictemplate="""
Write a summary of the following speech:
Speech:{speech}
Translate the precise summary to {language}
"""

prompt=PromptTemplate(
    input_variables=['speech','language'],
    template=generictemplate
)
prompt

llm_chain=LLMChain(llm=llm,prompt=prompt)
summary=llm_chain.run({'speech':speech,'language':'hindi'})
summary






```
