# 🟢 LangServe Code - 1

* uvicorn app:app --reload
* endpoints exposed
  * POST /qa/invoke&#x20;
  * POST /qa/batch&#x20;
  * GET /qa/stream

```python
# 2. app.py
from fastapi import FastAPI
from langserve import add_routes
from langchain.prompts import PromptTemplate
from langchain_openai import ChatOpenAI

app = FastAPI()

llm = ChatOpenAI(model="gpt-3.5-turbo")

prompt = PromptTemplate(
    input_variables=["question"],
    template="Answer briefly: {question}"
)

chain = prompt | llm

add_routes(
    app,
    chain,
    path="/qa"
)

```
