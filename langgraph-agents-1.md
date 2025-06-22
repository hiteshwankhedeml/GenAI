# LangGraph Agents

* &#x20;Docstring is important for LLM to understand the tool
* We also want the tool should not get executed parallelly - This needs to be specified during bind tools
* LLM will also have some functionality that we will define using system prompt
* Messagestate functionality is also available in langchain, so we can use inbuilt functionality instead of manually defining also
* Assistant function is for functionality of the chatbot
*
*

```python
import os
from dotenv import load_dotenv
load_dotenv()

os.environ["GROQ_API_KEY"]=os.getenv("GROQ_API_KEY")
os.environ["OPENAI_API_KEY"]=os.getenv("OPENAI_API_KEY")

from langchain_openai import ChatOpenAI

def multiply(a: int, b: int) -> int:
    """Multiply a and b.

    Args:
        a: first int
        b: second int
    """
    return a * b

# This will be a tool
def add(a: int, b: int) -> int:
    """Adds a and b.

    Args:
        a: first int
        b: second int
    """
    return a + b

def divide(a: int, b: int) -> float:
    """Divide a and b.

    Args:
        a: first int
        b: second int
    """
    return a / b

tools=[add,multiply,divide]

llm=ChatOpenAI(model="gpt-4o")

llm_with_tools=llm.bind_tools(tools,parallel_tool_calls=False)

from langgraph.graph import MessagesState

from typing_extensions import TypedDict
from langchain_core.messages import AnyMessage
from typing import Annotated
from langgraph.graph.message import add_messages

class MessagesState(TypedDict):
    messages:Annotated[list[AnyMessage],add_messages]

from langchain_core.messages import HumanMessage, SystemMessage

# System message
sys_msg = SystemMessage(content="You are a helpful assistant tasked with performing arithmetic on a set of inputs.")


def assistant(state:MessagesState):
    return {"messages":[llm_with_tools.invoke([sys_msg] + state["messages"])]}

from langgraph.graph import START, StateGraph
from langgraph.prebuilt import tools_condition
from langgraph.prebuilt import ToolNode
from IPython.display import Image, display


builder=StateGraph(MessagesState)

## Define the node
builder.add_node("assistant",assistant)
builder.add_node("tools",ToolNode(tools))

## Define the edges

builder.add_edge(START,"assistant")
builder.add_conditional_edges(
    "assistant",
    # If the latest message (result) from assistant is a tool call -> tools_condition routes to tools
    # If the latest message (result) from assistant is a not a tool call -> tools_condition routes to END
    tools_condition,
)

builder.add_edge("tools","assistant")

react_graph=builder.compile()

# Show
display(Image(react_graph.get_graph().draw_mermaid_png()))

messages = [HumanMessage(content="Add 10 and 14. Multiply the output by 2. Divide the output by 5")]
messages = react_graph.invoke({"messages": messages})
for m in messages['messages']:
    m.pretty_print()
#================================ Human Message =================================
#Add 10 and 14. Multiply the output by 2. Divide the output by 5
#================================== Ai Message ==================================
#Tool Calls:
#  add (call_tvIN1Wda1bfyfkZG6aVD3rSY)
# Call ID: call_tvIN1Wda1bfyfkZG6aVD3rSY
#  Args:
#    a: 10
#    b: 14
#================================= Tool Message =================================
#Name: add
#24
#================================== Ai Message ==================================
#Tool Calls:
#  multiply (call_JbXnUm9EzEpBr5DZNBQDLwhG)
# Call ID: call_JbXnUm9EzEpBr5DZNBQDLwhG
#  Args:
#    a: 24
#    b: 2
#================================= Tool Message =================================
#Name: multiply
#48
#================================== Ai Message ==================================
#Tool Calls:
#  divide (call_YVNuo2oDPBhbwZ94pVmZSMRK)
# Call ID: call_YVNuo2oDPBhbwZ94pVmZSMRK
#  Args:
#    a: 48
#    b: 5
#================================= Tool Message =================================
#Name: divide
#9.6
#================================== Ai Message ==================================
#The result of adding 10 and 14, multiplying the sum by 2, and then dividing by 5 is 9.6.    
```

*

    <figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
