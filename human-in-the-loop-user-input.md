# Human in the loop - User input

* &#x20;

```python
# System message
sys_msg = SystemMessage(content="You are a helpful assistant tasked with performing arithmetic on a set of inputs.")

# no-op node that should be interrupted on
def human_feedback(state: MessagesState):
    pass

# Assistant node
def assistant(state: MessagesState):
   return {"messages": [llm_with_tools.invoke([sys_msg] + state["messages"])]}

# Graph
builder = StateGraph(MessagesState)

# Define nodes: these do the work
builder.add_node("assistant", assistant)
builder.add_node("tools", ToolNode(tools))
builder.add_node("human_feedback", human_feedback)

# Define edges: these determine the control flow
builder.add_edge(START, "human_feedback")
builder.add_edge("human_feedback", "assistant")
builder.add_conditional_edges(
    "assistant",
    # If the latest message (result) from assistant is a tool call -> tools_condition routes to tools
    # If the latest message (result) from assistant is a not a tool call -> tools_condition routes to END
    tools_condition,
)
builder.add_edge("tools", "human_feedback")

memory = MemorySaver()
graph = builder.compile(interrupt_before=["human_feedback"], checkpointer=memory)
display(Image(graph.get_graph().draw_mermaid_png()))

# Input
initial_input = {"messages": "Multiply 2 and 3"}

# Thread
thread = {"configurable": {"thread_id": "5"}}

# Run the graph until the first interruption
for event in graph.stream(initial_input, thread, stream_mode="values"):
    event["messages"][-1].pretty_print()


## get user input

user_input=input("Tell me how you want to update the state:")

graph.update_state(thread,{"messages":user_input},as_node="human_feedback")

# Continue the graph execution
for event in graph.stream(None, thread, stream_mode="values"):
    event["messages"][-1].pretty_print()
#================================ Human Message =================================
#Multiply 2 and 3
#================================ Human Message =================================
#no multiply 4 and 3
#================================== Ai Message ==================================
#Tool Calls:
#  multiply (call_1gd3)
# Call ID: call_1gd3
#  Args:
#    a: 4
#    b: 3
#================================= Tool Message =================================
#Name: multiply
#12

for event in graph.stream(None, thread, stream_mode="values"):
    event["messages"][-1].pretty_print()
#================================= Tool Message =================================
#Name: multiply
#12
#================================== Ai Message ==================================
#The multiplication of 4 and 3 is 12.
```

*

    <figure><img src=".gitbook/assets/{7519C121-D0AB-4C81-BEEB-F7688FFB48A4}.png" alt=""><figcaption></figcaption></figure>
