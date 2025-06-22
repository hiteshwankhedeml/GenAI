# Memory in Agents using MemorySaver

* &#x20;Using memorysaver we can save every state/output
* After every interaction, checkpoint will be updated
* For different users, use different thread ids
* We can also save checkpoint in DB

```python
from langgraph.checkpoint.memory import MemorySaver

memory=MemorySaver()

react_graph=builder.compile(checkpointer=memory)

config={"configurable":{"thread_id":"1"}}
# Specify an input
messages = [HumanMessage(content="Add 3 and 4.")]

# Run
messages = react_graph.invoke({"messages": messages},config)
for m in messages['messages']:
    m.pretty_print()
#================================ Human Message =================================
#Add 3 and 4.
#================================== Ai Message ==================================
#Tool Calls:
#  add (call_Q89VgwgrSpj19HlYZ4oLxM6E)
# Call ID: call_Q89VgwgrSpj19HlYZ4oLxM6E
#  Args:
#    a: 3
#    b: 4
#================================= Tool Message =================================
#Name: add
#7
#================================== Ai Message ==================================
#The sum of 3 and 4 is 7.

messages = [HumanMessage(content="Multiply that by 2.")]
messages = react_graph.invoke({"messages": messages}, config)
for m in messages['messages']:
    m.pretty_print()
#================================ Human Message =================================
#Add 3 and 4.
#================================== Ai Message ==================================
#Tool Calls:
#  add (call_Q89VgwgrSpj19HlYZ4oLxM6E)
# Call ID: call_Q89VgwgrSpj19HlYZ4oLxM6E
#  Args:
#    a: 3
#    b: 4
#================================= Tool Message =================================
#Name: add
#7
#================================== Ai Message ==================================
#The sum of 3 and 4 is 7.
#================================ Human Message =================================
#Multiply that by 2.
#================================== Ai Message ==================================
#Tool Calls:
#  multiply (call_dJ1Q1nLlI5Z6xrXHSVgk2Bhr)
# Call ID: call_dJ1Q1nLlI5Z6xrXHSVgk2Bhr
#  Args:
#    a: 7
#    b: 2
#================================= Tool Message =================================
#Name: multiply
#14
#================================== Ai Message ==================================
#The result of multiplying 7 by 2 is 14.    

```
