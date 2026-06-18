# Code - Calling Different Models

* Open AI:&#x20;

```python
### Method 1:
from langchain.chat_models import init_chat_model

model=init_chat_model("gpt-4.1")
response=model.invoke("Hello How are you?")
response.content

### Method 2:
from langchain_openai import ChatOpenAI

model=ChatOpenAI(model="gpt-4.1")
response=model.invoke("Hello How are you?")
```

* Gemini:

```python
from langchain.chat_models import init_chat_model
model = init_chat_model("google_genai:gemini-2.5-flash")
response = model.invoke("Why do parrots talk?")

from langchain_google_genai import ChatGoogleGenerativeAI
model = ChatGoogleGenerativeAI(model="gemini-2.5-flash-lite")
response = model.invoke("Why do parrots talk?")
```

* Groq:

```python
from langchain.chat_models import init_chat_model
model = init_chat_model("groq:qwen/qwen3-32b")
response = model.invoke("Why do parrots talk?")

from langchain_groq import ChatGroq
model = ChatGroq(model="qwen/qwen3-32b")
response = model.invoke("Why do parrots talk?")
```
