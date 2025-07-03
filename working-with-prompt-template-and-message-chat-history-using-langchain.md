# 🟢 Working with Prompt template and Message Chat History Using LangChain

Prompt Templates:

* Help to turn raw user information into a format that the LLM can work with. In this case, the raw user input is just a message, which we are passing to the LLM.

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Create a chat prompt template**</mark>
* <mark style="color:purple;background-color:purple;">**Create a chain of prompt and model**</mark>
* <mark style="color:purple;background-color:purple;">**Create a runnable\_message\_history using chain and function to get message history**</mark>
* <mark style="color:purple;background-color:purple;">**Invoke the runnable**</mark>

```python
rom langchain_core.prompts import ChatPromptTemplate,MessagesPlaceholder
prompt=ChatPromptTemplate.from_messages(
    [
        ("system","You are a helpful assistant.Amnswer all the question to the best of your ability"),
        MessagesPlaceholder(variable_name="messages")
    ]
)

chain=prompt|model

chain.invoke({"messages":[HumanMessage(content="Hi My name is Krish")]})

with_message_history=RunnableWithMessageHistory(chain,get_session_history)

config = {"configurable": {"session_id": "chat3"}}
response=with_message_history.invoke(
    [HumanMessage(content="Hi My name is Krish")],
    config=config
)
response

response = with_message_history.invoke(
    [HumanMessage(content="What's my name?")],
    config=config,
)
response.content

##### Adding some complexity ######
prompt = ChatPromptTemplate.from_messages(
    [
        (
            "system",
            "You are a helpful assistant. Answer all questions to the best of your ability in {language}.",
        ),
        MessagesPlaceholder(variable_name="messages"),
    ]
)

chain = prompt | model

response=chain.invoke({"messages":[HumanMessage(content="Hi My name is Krish")],"language":"Hindi"})
response.content

with_message_history=RunnableWithMessageHistory(
    chain,
    get_session_history,
    input_messages_key="messages"
)

with_message_history=RunnableWithMessageHistory(
    chain,
    get_session_history,
    input_messages_key="messages"
)

config = {"configurable": {"session_id": "chat4"}}
repsonse=with_message_history.invoke(
    {'messages': [HumanMessage(content="Hi,I am Krish")],"language":"Hindi"},
    config=config
)
repsonse.content

response = with_message_history.invoke(
    {"messages": [HumanMessage(content="whats my name?")], "language": "Hindi"},
    config=config,
)
response.content
```
