# 🟢 LangSmith

* We can specify project and langsmith key in our project and enable tracing
* We can go into langsmith and see the entire trace like prompt, output etc

```python
## Langsmith Tracking and tracing

os.environ["LANGCHAIN_API_KEY"]=os.getenv("LANGCHAIN_API_KEY")
os.environ["LANGCHAIN_TRACING_V2"]="true"
os.environ["LANGCHAIN_PROJECT"]=os.getenv("LANGCHAIN_PROJECT")
```
