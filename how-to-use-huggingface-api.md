# ✈️ How to use HuggingFace API

<mark style="color:purple;background-color:purple;">**How to access the huggingface API:**</mark>

* There are also two ways to use this class. You can specify the model with the repo\_id parameter.
* <mark style="color:purple;background-color:purple;">**Those endpoints use the serverless API, which is particularly beneficial to people using pro accounts or enterprise hub.**</mark>
* Still, regular users can already have access to a fair amount of request by connecting with their HF token in the environment where they are executing the code
* <mark style="color:purple;background-color:purple;">**We need to use HuggingFaceEndpoint - Pass repo, key etc**</mark>

```python
repo_id="mistralai/Mistral-7B-Instruct-v0.3"
llm=HuggingFaceEndpoint(repo_id=repo_id,max_length=150,temperature=0.7,token=hf_api_key)
```
