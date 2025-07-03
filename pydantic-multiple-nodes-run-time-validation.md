# ⚔️ Pydantic - Multiple Nodes- Run time validation

* Run-time validation will also work in a multi-node graph. In the example below bad\_node updates a to an integer.
* Because run-time validation occurs on inputs, the validation error will occur when ok\_node is called (not when bad\_node returns an update to the state which is inconsistent with the schema).

```python
from langgraph.graph import StateGraph, START, END
from typing_extensions import TypedDict

from pydantic import BaseModel


class OverallState(BaseModel):
    a:str

def bad_node(state: OverallState):
    return {
        "a": 123  # Invalid
    }

def ok_node(state:OverallState):
    return {"a":"goodbye"}

# Build the state graph
builder = StateGraph(OverallState)
builder.add_node(bad_node)
builder.add_node(ok_node)
builder.add_edge(START, "bad_node")
builder.add_edge("bad_node", "ok_node")
builder.add_edge("ok_node", END)
graph = builder.compile()

# Test the graph with a valid input
try:
    graph.invoke({"a": "krish"})
except Exception as e:
    print("An exception was raised because bad_node sets `a` to an integer.")
    print(e)
#An exception was raised because bad_node sets `a` to an integer.
#1 validation error for OverallState
#a
#  Input should be a valid string [type=string_type, input_value=123, input_type=int]
#    For further information visit https://errors.pydantic.dev/2.10/v/string_type


```
