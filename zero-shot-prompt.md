# 🟢 Zero Shot Prompt

* <mark style="color:purple;background-color:purple;">**In zero-shot prompting, the model receives no examples — only a clear instruction.**</mark>
* <mark style="color:purple;background-color:purple;">**Relies on the model’s pretraining to understand and complete the task.**</mark>
* Works best for common, generic tasks like translation, classification, summarization, extraction, etc.

```python
prompt_template = PromptTemplate(
    input_variables=["input_text"],
    template="""Extract all person names mentioned in the following text:

Text: "{{input_text}}"
Person Names:"""
)

chain = LLMChain(llm=llm, prompt=prompt_template)
output = chain.run("Steve Jobs and Elon Musk were both visionary leaders.")
print(output)

```
