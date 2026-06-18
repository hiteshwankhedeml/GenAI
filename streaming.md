# Streaming

* Most models can stream their output content while it is being generated.&#x20;
* By displaying output progressively, streaming significantly improves user experience, particularly for longer responses.&#x20;
* Calling stream() returns an iterator that yields output chunks as they are produced.&#x20;
* You can use a loop to process each chunk in real-time

```python
for chunk in model.stream("Write me a 200 words paragraph on Artificial Intelligence"):
    print(chunk.text, end="|", flush=True)
```
