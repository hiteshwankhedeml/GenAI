# 🟢 Building Chatbots with Message History

* We will be using Groq API here
* Human message means its a user message
* When we send Human message ⇒ LLM will respond with AI Message
* Runnable means it will help to run in the chain
* <mark style="color:purple;background-color:purple;">**BaseChatMessageHistory is abstract class for storing message history**</mark>
* <mark style="color:purple;background-color:purple;">**We can save history in db also, here we are using in memory**</mark>
* Message History:
  * We can use a Message History class to wrap our model and make it stateful.
  * This will keep track of inputs and outputs of the model, and store them in some datastore.
  * Future interactions will then load those messages and pass them into the chain as part of the input. Let's see how to use this!
  * ChatMessageHistory means a single message
  * BaseChatMessageHistory means entire history
  * Config is for session

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Keep a dictionary called state, it will elements as session\_id(key) and message\_history(value)**</mark>
* <mark style="color:purple;background-color:purple;">**We will have a function in which if we pass session\_id, it will give us back message\_history for that session\_id**</mark>
* <mark style="color:purple;background-color:purple;">**If not found in state, then it will create an element in state**</mark>
* <mark style="color:purple;background-color:purple;">**We will create a runnable with the model and the function**</mark>
* <mark style="color:purple;background-color:purple;">**in a config, pass session\_id**</mark>
* <mark style="color:purple;background-color:purple;">**invoke the runnable with human message and config**</mark>

```python
import os
from dotenv import load_dotenv
load_dotenv() ## aloading all the environment variable

groq_api_key=os.getenv("GROQ_API_KEY")
groq_api_key


from langchain_groq import ChatGroq
model=ChatGroq(model="Gemma2-9b-It",groq_api_key=groq_api_key)
model

from langchain_core.messages import HumanMessage
model.invoke([HumanMessage(content="Hi , My name is Krish and I am a Chief AI Engineer")])
# AIMessage(content="Hi Krish, it's nice to meet you!\n\nThat's a fascinating role. 
# What kind of projects are you working on as Chief AI Engineer? Are you focused on a
# particular industry or application of AI? I'm always eager to learn more about how 
# AI is being used in the world.\n", additional_kwargs={}, 
# response_metadata={'token_usage': {'completion_tokens': 65, 'prompt_tokens': 22, 
# 'total_tokens': 87, 'completion_time': 0.118181818, 'prompt_time': 0.00013921, 
# 'queue_time': 0.023974065, 'total_time': 0.118321028}, 'model_name': 'Gemma2-9b-It', 
# 'system_fingerprint': 'fp_10c08bf97d', 'finish_reason': 'stop', 'logprobs': None}, 
# id='run-b0183b99-352a-4dd8-ba63-40d09690a986-0', usage_metadata={'input_tokens': 22,
# 'output_tokens': 65, 'total_tokens': 87})


from langchain_core.messages import AIMessage
model.invoke(
    [
        HumanMessage(content="Hi , My name is Krish and I am a Chief AI Engineer"),
        AIMessage(content="Hello Krish! It's nice to meet you. \n\nAs a Chief AI Engineer, what kind of projects are you working on these days? \n\nI'm always eager to learn more about the exciting work being done in the field of AI.\n"),
        HumanMessage(content="Hey What's my name and what do I do?")
    ]
)
# Here it will be able to respond because it is having context in the form of message
# history


from langchain_community.chat_message_histories import ChatMessageHistory
from langchain_core.chat_history import BaseChatMessageHistory
from langchain_core.runnables.history import RunnableWithMessageHistory

store={}

def get_session_history(session_id:str)->BaseChatMessageHistory:
    if session_id not in store:
        store[session_id]=ChatMessageHistory()
    return store[session_id]

with_message_history=RunnableWithMessageHistory(model,get_session_history)

config={"configurable":{"session_id":"chat1"}}

response=with_message_history.invoke(
    [HumanMessage(content="Hi , My name is Krish and I am a Chief AI Engineer")],
    config=config
)

response.content
#"Hi Krish!\n\nIt's great to meet you.  Being a Chief AI Engineer is 
# a fascinating role. What kind of projects are you currently working on?  
# I'm always interested in hearing about the cutting edge of AI development.  \n\n"

with_message_history.invoke(
    [HumanMessage(content="What's my name?")],
    config=config,
)

## change the config-->session id
config1={"configurable":{"session_id":"chat2"}}
response=with_message_history.invoke(
    [HumanMessage(content="Whats my name")],
    config=config1
)
response.content
# "As an AI, I have no memory of past conversations and don't know your name. 
# If you'd like to tell me, I'd be happy to use it! 😊  \n\n"

```
