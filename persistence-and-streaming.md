# 🟢 Persistence and Streaming

* <mark style="color:purple;background-color:purple;">**For longer running tasks, persistence and streaming are very important**</mark>

<mark style="color:purple;background-color:purple;">**Persistence:**</mark>

* <mark style="color:purple;background-color:purple;">**Keep around the state of an agent at a particular point in time**</mark>
* <mark style="color:purple;background-color:purple;">**This can let go back to that state and resume in that state in future interactions**</mark>
* <mark style="color:purple;background-color:purple;">**This is very important for long running applications**</mark>
* <mark style="color:purple;background-color:purple;">**This has been achieved using checkpointer**</mark>
* <mark style="color:purple;background-color:purple;">**Checkpointer checks the state after and between every nide**</mark>

<mark style="color:purple;background-color:purple;">**Streaming:**</mark>

* <mark style="color:purple;background-color:purple;">**To emit what's going on at the exact moment**</mark>
* <mark style="color:purple;background-color:purple;">**For streaming tokens we will have to use async**</mark>
* <mark style="color:purple;background-color:purple;">**Used for debugging also**</mark>

<mark style="color:purple;background-color:purple;">**Code:**</mark>

* <mark style="color:purple;background-color:purple;">**Create a checkpoint of sqlite with inmemory**</mark>
* <mark style="color:purple;background-color:purple;">**When you compile add checkpointer**</mark>
* <mark style="color:purple;background-color:purple;">**During graph.stream ⇒ pass human message and thread id**</mark>
* <mark style="color:purple;background-color:purple;">**using thread different coversation can be managed**</mark>
* <mark style="color:purple;background-color:purple;">**We can also stream tokens, for that we will have to use async checkpoint and astream**</mark>

```python
from dotenv import load_dotenv

_ = load_dotenv()

from langgraph.graph import StateGraph, END
from typing import TypedDict, Annotated
import operator
from langchain_core.messages import AnyMessage, SystemMessage, HumanMessage, ToolMessage
from langchain_openai import ChatOpenAI
from langchain_community.tools.tavily_search import TavilySearchResults

tool = TavilySearchResults(max_results=2)

class AgentState(TypedDict):
    messages: Annotated[list[AnyMessage], operator.add]

from langgraph.checkpoint.sqlite import SqliteSaver

memory = SqliteSaver.from_conn_string(":memory:")

class Agent:
    def __init__(self, model, tools, checkpointer, system=""):
        self.system = system
        graph = StateGraph(AgentState)
        graph.add_node("llm", self.call_openai)
        graph.add_node("action", self.take_action)
        graph.add_conditional_edges("llm", self.exists_action, {True: "action", False: END})
        graph.add_edge("action", "llm")
        graph.set_entry_point("llm")
        self.graph = graph.compile(checkpointer=checkpointer)
        self.tools = {t.name: t for t in tools}
        self.model = model.bind_tools(tools)

    def call_openai(self, state: AgentState):
        messages = state['messages']
        if self.system:
            messages = [SystemMessage(content=self.system)] + messages
        message = self.model.invoke(messages)
        return {'messages': [message]}

    def exists_action(self, state: AgentState):
        result = state['messages'][-1]
        return len(result.tool_calls) > 0

    def take_action(self, state: AgentState):
        tool_calls = state['messages'][-1].tool_calls
        results = []
        for t in tool_calls:
            print(f"Calling: {t}")
            result = self.tools[t['name']].invoke(t['args'])
            results.append(ToolMessage(tool_call_id=t['id'], name=t['name'], content=str(result)))
        print("Back to the model!")
        return {'messages': results}

prompt = """You are a smart research assistant. Use the search engine to look up information. \
You are allowed to make multiple calls (either together or in sequence). \
Only look up information when you are sure of what you want. \
If you need to look up some information before asking a follow up question, you are allowed to do that!
"""
model = ChatOpenAI(model="gpt-4o")
abot = Agent(model, [tool], system=prompt, checkpointer=memory)

messages = [HumanMessage(content="What is the weather in sf?")]

thread = {"configurable": {"thread_id": "1"}}

for event in abot.graph.stream({"messages": messages}, thread):
    for v in event.values():
        print(v['messages'])

messages = [HumanMessage(content="What about in la?")]
thread = {"configurable": {"thread_id": "1"}}
for event in abot.graph.stream({"messages": messages}, thread):
    for v in event.values():
        print(v)

messages = [HumanMessage(content="Which one is warmer?")]
thread = {"configurable": {"thread_id": "1"}}
for event in abot.graph.stream({"messages": messages}, thread):
    for v in event.values():
        print(v)


### Below we are using different thread id, so it wont understand
messages = [HumanMessage(content="Which one is warmer?")]
thread = {"configurable": {"thread_id": "2"}}
for event in abot.graph.stream({"messages": messages}, thread):
    for v in event.values():
        print(v)


### Streaming tokens
from langgraph.checkpoint.aiosqlite import AsyncSqliteSaver

memory = AsyncSqliteSaver.from_conn_string(":memory:")
abot = Agent(model, [tool], system=prompt, checkpointer=memory)

messages = [HumanMessage(content="What is the weather in SF?")]
thread = {"configurable": {"thread_id": "4"}}
async for event in abot.graph.astream_events({"messages": messages}, thread, version="v1"):
    kind = event["event"]
    if kind == "on_chat_model_stream":
        content = event["data"]["chunk"].content
        if content:
            # Empty content in the context of OpenAI means
            # that the model is asking for a tool to be invoked.
            # So we only print non-empty content
            print(content, end="|")
# Calling: {'name': 'tavily_search_results_json', 'args': 
# {'query': 'current weather in San Francisco'}, 'id': 'call_GiHnpbt7P6kuX4g2n6imqDXI'}
# /usr/local/lib/python3.11/site-packages/langchain_core/_api/beta_decorator.py:87:
# LangChainBetaWarning: This API is in beta and may change in the future.
#  warn_beta(
# Back to the model!
# The| current| weather| in| San| Francisco| is| mostly| sunny| with| a| high| of|
# |69|°F| (|approximately| |20|.|5|°C|).| Winds| are| coming| from| the| west| at|
# |15| to| |25| mph|.| During| the| night|,| partly| cloudy| skies| are| expected|
# with| a| low| around| |60|°F| (|approximately| |15|.|5|°C|)| and| winds| from| the|
# west| at| |10| to| |20| mph|.|

```
