# Batch

* Batching a collection of independent requests to a model can significantly improve performance and reduce costs, as the processing can be done in parallel
* If we pass 3 query, then we will get 3 content in response
* max\_concurrency ⇒ If we set concurrency as 5, and give 10 inputs then it will call LLM twice

```python
model.batch(
    ["Why do parrots have colorful feathers?",
    "How do airplanes fly?",
    "What is quantum computing?"],
    config={
        'max_concurrency': 5,  # Limit to 5 parallel calls
    }
)
```
