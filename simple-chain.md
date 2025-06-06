# Simple Chain

* &#x20;For generating response we need - Prompt + LLM + Output Parser
* pip install grandalf to visualize the chain

```python
from langchain_core.output_parsers import StrOutputParser

parser=StrOutputParser()

prompt=PromptTemplate(
    template="can you give me a detail explaination of {topic}",
    input_variables=['topic']
    
)

chain=prompt | openai_model | parser
chain.invoke({"topic":"machine learning"})
chain.get_graph().print_ascii()
```

*

    <figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
