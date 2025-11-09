# 🔴 Pydantic - Data Type Validation

* Here class state with return dictionary of 2 variables of str
* If we initialize the class and don't give the string values then also it will work

```python
from typing_extensions import TypedDict

class State(TypedDict):
    xyz:str
    abc:str
```

* &#x20;Pydantic is for validation of data used inside the class
* We define basemodel class and then use it inside the node
* Then its mandatory to pass string here, otherwise it will give error

```python
from langgraph.graph import StateGraph, START, END
from typing_extensions import TypedDict

from pydantic import BaseModel


# The overall state of the graph (this is the public state shared across nodes)
class OverallState(BaseModel):
    a: str

def node(state:OverallState):
    return {"a":"Hi I am Krish"}

# Build the state graph
builder = StateGraph(OverallState)
builder.add_node(node)  # node_1 is the first node
builder.add_edge(START, "node")  # Start the graph with node_1
builder.add_edge("node", END)  # End the graph after node_1
graph = builder.compile()

graph.invoke({"a":"Hello"})
# {'a': 'Hi I am Krish'}


```
