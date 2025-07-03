# 🟢 Managing the Chat Conversion History

* If message history is left unmanaged, the list of messages will grow unbounded and potentially overflow the context window of the LLM.
* Therefore, it is important to add a step that limits the size of the messages you are passing in. 'trim\_messages' helper to reduce how many messages we're sending to the model.
* The trimmer allows us to specify how many tokens we want to keep, along with other parameters like if we want to always keep the system message and whether to allow partial messages

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Create a trimmer ⇒ This has different parameters like no. of messages or no. of tokens etc**</mark>
* <mark style="color:purple;background-color:purple;">**Create a chain of trimmer | model | prompt**</mark>
* <mark style="color:purple;background-color:purple;">**Create a runnable\_chain\_history using the chain and function to get message history**</mark>

```python
from langchain_core.messages import SystemMessage,trim_messages
trimmer=trim_messages(
    max_tokens=45,
    strategy="last",
    token_counter=model,
    include_system=True,
    allow_partial=False,
    start_on="human"
)
messages = [
    SystemMessage(content="you're a good assistant"),
    HumanMessage(content="hi! I'm bob"),
    AIMessage(content="hi!"),
    HumanMessage(content="I like vanilla ice cream"),
    AIMessage(content="nice"),
    HumanMessage(content="whats 2 + 2"),
    AIMessage(content="4"),
    HumanMessage(content="thanks"),
    AIMessage(content="no problem!"),
    HumanMessage(content="having fun?"),
    AIMessage(content="yes!"),
]
trimmer.invoke(messages)
# Here we will be the messages in the message history

from operator import itemgetter
from langchain_core.runnables import RunnablePassthrough

chain=(
    RunnablePassthrough.assign(messages=itemgetter("messages")|trimmer)
    | prompt
    | model
    
)

response=chain.invoke(
    {
    "messages":messages + [HumanMessage(content="What ice cream do i like")],
    "language":"English"
    }
)
response.content
# "As an AI, I don't have access to your personal preferences
# like your favorite ice cream flavor.  \n\nWhat's your favorite ice cream? 😊🍦\n"

response = chain.invoke(
    {
        "messages": messages + [HumanMessage(content="what math problem did i ask")],
        "language": "English",
    }
)
response.content
# 'You asked "whats 2 + 2" 😊  \n\n\n\n'

## Lets wrap this in the MEssage History
with_message_history = RunnableWithMessageHistory(
    chain,
    get_session_history,
    input_messages_key="messages",
)
config={"configurable":{"session_id":"chat5"}}

response = with_message_history.invoke(
    {
        "messages": messages + [HumanMessage(content="whats my name?")],
        "language": "English",
    },
    config=config,
)

response.content
# "As a large language model, I don't have access to
#  past conversations or any personal information about you, 
# including your name.  \n\nIf you'd like to tell me your name, I'd be happy to know! 😊  \n\n"

response = with_message_history.invoke(
    {
        "messages": [HumanMessage(content="what math problem did i ask?")],
        "language": "English",
    },
    config=config,
)
response.content
# "As a large language model, I have no memory
# of past conversations. If you'd like to ask me a math problem, 
# I'm happy to help! 😊  Just let me know what it is. \n\n"
```
