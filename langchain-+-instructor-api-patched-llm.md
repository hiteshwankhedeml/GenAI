# 🟢 LangChain + Instructor API (Patched LLM)

* <mark style="color:purple;background-color:purple;">**While initializing the llm we just need to use patch here, that's all**</mark>

```python
from instructor import patch
from pydantic import BaseModel
from langchain_openai import ChatOpenAI

# Schema
class UserInput(BaseModel):
    name: str
    age: int

# Patch the LLM
llm = patch(ChatOpenAI(model="gpt-4o-mini", temperature=0))

resp = llm.invoke("My name is Hitesh and I am 38 years old", response_model=UserInput)
print(resp)
# ✅ Always returns UserInput(name='Hitesh', age=38)
# ✅ Instructor retries/fixes automatically if format is wrong
```
