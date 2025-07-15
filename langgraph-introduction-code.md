# LangGraph Introduction - Code

* Using LangGraph we can do orchestration
* Orchestration means to create pipeline, to connect different components
* Node represents function
* Edge represents connectivity
* We can use app.stream to see the output of each node
* We can do sequential as well as parallel node execution

```python
from langgraph.graph import Graph
from langgraph.graph import StateGraph
from dotenv import load_dotenv
load_dotenv()

from langchain_google_genai import GoogleGenerativeAIEmbeddings
embeddings = GoogleGenerativeAIEmbeddings(model="models/embedding-001")
from langchain_google_genai import ChatGoogleGenerativeAI
llm = ChatGoogleGenerativeAI(model="gemini-1.5-pro")

def LLM(input):
    llm = ChatGoogleGenerativeAI(model="gemini-1.5-pro")
    response=llm.invoke(input).content
    return response

def Counter_Token(input):
    token=input.split()
    token_number=len(token)
    response=f"total number of token in the generated output {token_number}"
    return response

workflow=Graph()

workflow.add_node("MY LLM",LLM)
workflow.add_node("Token Counter",Counter_Token)

workflow.add_edge("MY LLM", "Token Counter")

workflow.set_entry_point("MY LLM")
workflow.set_finish_point("Token Counter")

app=workflow.compile()

from IPython.display import Image, display
app.get_graph()
display(Image(app.get_graph().draw_mermaid_png())) 

app.invoke("what is a agentic ai explain me in very detailed manner?")
# 'total number of token in the generated output 584'

for output in app.stream("what is a agentic ai explain me in very detailed manner?"):
    for key,value in output.items():
        print(f"here is output from {key}")
        print("_______")
        print(value)
        print("\n")
```

*

    <figure><img src=".gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>
