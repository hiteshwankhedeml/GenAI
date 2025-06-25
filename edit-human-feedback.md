# Edit Human Feedback

* &#x20;

```python
initial_input={"messages":HumanMessage(content="Multiply 2 and 3")}

thread={"configurable":{"thread_id":"1"}}
for event in graph.stream(initial_input,thread,stream_mode="values"):
    event['messages'][-1].pretty_print()
    
# ================================ Human Message =================================
# Multiply 2 and 3

graph.update_state(thread,{"messages":[HumanMessage(content="No, actually multiply 15 and 5!")]})
# {'configurable': {'thread_id': '1',
#  'checkpoint_ns': '',
#  'checkpoint_id': '1eff1d3c-4c12-610e-8006-7362bf29d06f'}}

new_state = graph.get_state(thread).values
for m in new_state['messages']:
    m.pretty_print()
# ================================ Human Message =================================
# Multiply 2 and 3
# ================================== Ai Message ==================================
# Tool Calls:
#   multiply (call_pksn)
#  Call ID: call_pksn
#   Args:
#     a: 2
#     b: 3
# ================================= Tool Message =================================
# Name: multiply
# 6
# ================================== Ai Message ==================================
# The result of multiplying 2 and 3 is 6.
# ================================ Human Message =================================
# Multiply 2 and 3
# ================================ Human Message =================================
# No, actually multiply 15 and 5!

for event in graph.stream(None, thread, stream_mode="values"):
    event['messages'][-1].pretty_print()
# ================================ Human Message =================================
# No, actually multiply 15 and 5!
# ================================== Ai Message ==================================
# Tool Calls:
#   multiply (call_rha9)
#  Call ID: call_rha9
#   Args:
#     a: 15
#     b: 5
# ================================= Tool Message =================================
# Name: multiply
# 75

new_state = graph.get_state(thread).values
for m in new_state['messages']:
    m.pretty_print()
#================================ Human Message =================================
#Multiply 2 and 3
#================================= Ai Message ==================================
#Tool Calls:
#  multiply (call_pksn)
# Call ID: call_pksn
#  Args:
#    a: 2
#    b: 3
#================================= Tool Message =================================
#Name: multiply
#6
#================================== Ai Message ==================================
#The result of multiplying 2 and 3 is 6.
#================================ Human Message =================================
#Multiply 2 and 3
#=============================== Human Message =================================
#No, actually multiply 15 and 5!
#================================= Ai Message ==================================
#Tool Calls:
#  multiply (call_rha9)
# Call ID: call_rha9
#  Args:
#    a: 15
#    b: 5
#================================= Tool Message =================================
#Name: multiply
#75

state=graph.get_state(thread)
state.next
# ('assistant',)

for event in graph.stream(None, thread, stream_mode="values"):
    event['messages'][-1].pretty_print()
# ================================= Tool Message =================================
# Name: multiply
# 75
# ================================== Ai Message ==================================
# The product of 15 and 5 is 75.
```
