# 🟢 Refine Chain

* <mark style="color:purple;background-color:purple;">**We have documents ⇒ This will be divided into chunks**</mark>
* <mark style="color:purple;background-color:purple;">**We will take the 1st chain and give it to a prompt template and LLM and get summarization**</mark>
* <mark style="color:purple;background-color:purple;">**For the second chain, before sending it to prompt, we will take reference of summarization 1 will get it**</mark>
* <mark style="color:purple;background-color:purple;">**Update a rolling summary be iterating over the documents in a sequence**</mark>    &#x20;
* <mark style="color:purple;background-color:purple;">**chain\_type="refine"**</mark>
*

```
<figure><img src=".gitbook/assets/{1F6CB6D7-57A0-4A61-B845-D8CAD9066BB0}.png" alt=""><figcaption></figcaption></figure>
```

```python
chain=load_summarize_chain(
    llm=llm,
    chain_type="refine",
    verbose=True
)
output_summary=chain.run(final_documents)
output_summary
```
