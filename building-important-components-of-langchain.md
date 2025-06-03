# Building important Components of LangChain

* Since we have already set environment variables, so we dont need to explicity pass key for calling the API
* In LangSmith, we will be able to see all the request, time etc
* Prompt template is for what kind of role you want LLM app to play/behave
* Chain means to combine / design the flow

```python
import os
from dotenv import load_dotenv
load_dotenv()

os.environ['OPENAI_API_KEY']=os.getenv("OPENAI_API_KEY")
## Langsmith Tracking
os.environ["LANGCHAIN_API_KEY"]=os.getenv("LANGCHAIN_API_KEY")
os.environ["LANGCHAIN_TRACING_V2"]="true"
os.environ["LANGCHAIN_PROJECT"]=os.getenv("LANGCHAIN_PROJECT")

from langchain_openai import ChatOpenAI
llm=ChatOpenAI(model="gpt-4o")
print(llm)

result=llm.invoke("What is generative AI?")

## Input and get response form LLM
result=llm.invoke("What is generative AI?")
print(result)

### Chatprompt Template
from langchain_core.prompts import ChatPromptTemplate

prompt=ChatPromptTemplate.from_messages(
    [
        ("system","You are an expert AI Engineer. Provide me answers based on the questions"),
        ("user","{input}")
    ]
)

chain=prompt|llm

response=chain.invoke({"input":"Can you tell me about Langsmith?"})
print(response)

type(response)
#langchain_core.messages.ai.AIMessage

## stroutput Parser

from langchain_core.output_parsers import StrOutputParser
output_parser=StrOutputParser()
chain=prompt|llm|output_parser

response=chain.invoke({"input":"Can you tell me about Langsmith?"})
print(response)
```
