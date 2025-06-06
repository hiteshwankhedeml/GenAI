# 🟢 LangChain - Load Model

* We can load models from different sources like groq, google, anthropic etc
* GoogleAIstudio to get key for gemini
* [https://python.langchain.com/docs/integrations/providers/all/](https://python.langchain.com/docs/integrations/providers/all/)
* <mark style="color:purple;background-color:purple;">**Steps:**</mark>
  * <mark style="color:purple;background-color:purple;">Instantiate the model</mark>
  * <mark style="color:purple;background-color:purple;">model.invoke(prompt)</mark>

```python
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain_groq import ChatGroq
from langchain_openai import ChatOpenAI
#from langchain_anthropic import ChatAnthropic
from langchain_huggingface import ChatHuggingFace, HuggingFaceEndpoint

llm=HuggingFaceEndpoint(repo_id="TinyLlama/TinyLlama-1.1B-Chat-v1.0",task="text-generation")
model=ChatHuggingFace(llm=llm)
model.invoke("what is capital of USA?").content

gemini_model=ChatGoogleGenerativeAI(model='gemini-1.5-pro')
gemini_model.invoke("what is capital of USA?")

groq_model=ChatGroq(model="deepseek-r1-distill-llama-70b")
groq_model.invoke("what is capital of USA")

openai_model=ChatOpenAI(model="gpt-4")
openai_model.invoke("what is capital of USA")

from langchain_huggingface import ChatHuggingFace, HuggingFacePipeline

llm = HuggingFacePipeline.from_model_id(
    model_id='TinyLlama/TinyLlama-1.1B-Chat-v1.0',
    task='text-generation',
    pipeline_kwargs=dict(
        temperature=0.5,
        max_new_tokens=100
    )
)
model = ChatHuggingFace(llm=llm)




```
