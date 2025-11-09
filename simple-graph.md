# 🟢 Simple Graph

<mark style="color:purple;background-color:purple;">**State:**</mark>

* First, define the State of the graph.
* <mark style="color:purple;background-color:purple;">**The State schema serves as the input schema for all Nodes and Edges in the graph.**</mark>
* Let's use the TypedDict class from python's typing module as our schema, which provides type hints for the keys.

```python
from typing_extensions import TypedDict
class State(TypedDict):
    graph_state:str
```

<mark style="color:purple;background-color:purple;">**Nodes:**</mark>

* Nodes are just python functions.
* The first positional argument is the state, as defined above.
* Because the state is a TypedDict with schema as defined above, each node can access the key, graph\_state, with state\['graph\_state'].
* <mark style="color:purple;background-color:purple;">**Each node returns a new value of the state key graph\_state.**</mark>
* By default, the new value returned by each node will override the prior state value.

```python
def first_node(state):
    print("My First Node is called")
    return {"graph_state":state['graph_state']+"I am playing"}

def second_node(state):
    print("My Second Node is called")
    return {"graph_state":state['graph_state']+" Cricket"}


def third_node(state):
    print("My Third Node is called")
    return {"graph_state":state['graph_state']+" Badminton"}
```

**Edges:**

* Edges connect the nodes.
* Normal Edges are used if you want to always go from, for example, node\_1 to node\_2.
* Conditional Edges are used if you want to optionally route between nodes.
* Conditional edges are implemented as functions that return the next node to visit based upon some logic.

```python
import random
from typing import Literal

def decide_play(state)->Literal['second_node','third_node']:
    graph_state=state['graph_state']

    if random.random()<0.5:
        return "second_node"
    
    return "third_node"
```

**Graph Construction:**

* Now, we build the graph from our components defined above.
* The StateGraph class is the graph class that we can use.
* First, we initialize a StateGraph with the State class we defined above.
* Then, we add our nodes and edges.
* We use the START Node, a special node that sends user input to the graph, to indicate where to start our graph.
* The END Node is a special node that represents a terminal node.
* Finally, we compile our graph to perform a few basic checks on the graph structure.
* We can visualize the graph as a Mermaid diagram.

```python
from IPython.display import Image, display
from langgraph.graph import StateGraph, START, END

## Build Graph
builder=StateGraph(State)

builder.add_node("first_node",first_node)
builder.add_node("second_node",second_node)
builder.add_node("third_node",third_node)

## Logic
builder.add_edge(START,"first_node")
builder.add_conditional_edges("first_node",decide_play)
builder.add_edge("second_node",END)
builder.add_edge("third_node",END)

## Add
graph=builder.compile()

## View
display(Image(graph.get_graph().draw_mermaid_png()))
```

*

    <figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

Graph Invocation:

* The compiled graph implements the runnable protocol.
* This provides a standard way to execute LangChain components.
* invoke is one of the standard methods in this interface.
* The input is a dictionary {"graph\_state": "Hi, this is lance."}, which sets the initial value for our graph state dict.
* When invoke is called, the graph starts execution from the START node.
* It progresses through the defined nodes (node\_1, node\_2, node\_3) in order.
* The conditional edge will traverse from node 1 to node 2 or 3 using a 50/50 decision rule.
* Each node function receives the current state and returns a new value, which overrides the graph state.
* The execution continues until it reaches the END node.

```python
graph.invoke({"graph_state":"Hi,My name is Krish,"})
# My First Node is called
# My Second Node is called
# {'graph_state': 'Hi,My name is Krish,I am playing Cricket'}
```

