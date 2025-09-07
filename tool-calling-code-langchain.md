# Tool Calling Code - LangChain

**Pydantic’s role**

* Defines a schema (input contract).
* Ensures validation (correct types, required fields).
* Converts natural language → structured JSON inputs for tools.

**Why needed**

* LLMs generate text, but tools need structured inputs.
* Pydantic ensures the LLM provides inputs in the right format.

**Steps**

1. Define a **Pydantic model** for tool inputs.
2. Attach that schema to a function with `@tool`.
3. LangChain auto-generates JSON schema for the LLM.
4. LLM fills schema → LangChain validates → function executes.

**`@tool` decorator**

* Registers a function as a LangChain tool.
* Attaches name, description, and schema.
* Converts Pydantic model into JSON schema.
* Routes LLM’s structured output to the function call.
* Basically: **makes your Python function usable by the LLM**

```python
from pydantic import BaseModel, Field
from langchain.tools import tool
from langchain_openai import ChatOpenAI
from langchain.agents import initialize_agent, AgentType

# Step 1: Define schema
class CalculatorInput(BaseModel):
    a: int = Field(..., description="First number")
    b: int = Field(..., description="Second number")

# Step 2: Define tool
@tool("add_numbers", args_schema=CalculatorInput)
def add_numbers(inputs: CalculatorInput) -> str:
    """Add two numbers and return the result"""
    return str(inputs.a + inputs.b)

# Step 3: Initialize LLM
llm = ChatOpenAI(model="gpt-4o-mini", temperature=0)

# Step 4: Create agent with tool
agent = initialize_agent(
    tools=[add_numbers],
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

# Step 5: Ask a question
response = agent.run("Can you add 12 and 8?")
print(response)
```

#### 🔍 What happens internally:

* `@tool` turns `add_numbers` into a LangChain tool.
* Pydantic model → JSON schema → sent to LLM.
* LLM sees: _“I can call add\_numbers with {a, b}”_.
* LLM decides: `{"a": 12, "b": 8}`.
* LangChain validates via Pydantic → runs `add_numbers(12, 8)` → returns `20`

