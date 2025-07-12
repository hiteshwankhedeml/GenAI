# 🟢 Stuff Document Chain

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">Create a prompt for summarization</mark>
* <mark style="color:purple;background-color:purple;">Create chain using load\_summarize\_chain ⇒ pass llm, prompt, and chain\_type='stuff'</mark>
* <mark style="color:purple;background-color:purple;">Invoke the chain by passing the docs</mark>

```python
from langchain_community.document_loaders import PyPDFLoader

loader = PyPDFLoader("apjspeech.pdf")
docs = loader.load_and_split()
docs

template=""" Write a concise and short summary of the following speech,
Speech :{text}

 """
prompt=PromptTemplate(input_variables=['text'],
                      template=template)


from langchain.chains.summarize import load_summarize_chain

chain=load_summarize_chain(llm,chain_type='stuff',prompt=prompt,verbose=True)
output_summary=chain.run(docs)
output_summary
```
