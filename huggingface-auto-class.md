# 🟢 HuggingFace - Auto Class

* <mark style="color:purple;background-color:purple;">**Use AutoModelForSequenceClassification and AutoTokenizer to load the pretrained model and it’s associated tokenizer**</mark>
* <mark style="color:purple;background-color:purple;">**For every pre-trained model, vocab will be different ⇒ So for every model tokenizer will be different**</mark>
* <mark style="color:purple;background-color:purple;">**We need to use tokenizer which was used for pre-training the model, we cannot use normal tokenization**</mark>
* <mark style="color:purple;background-color:purple;">**By default, it also loads the model in memory**</mark>
* <mark style="color:purple;background-color:purple;">**We can also save and load the model locally**</mark>

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification

tokenizer=AutoTokenizer.from_pretrained("google/flan-t5-large")
model=AutoModelForSequenceClassification.from_pretrained("google/flan-t5-large")

#-----------------Saving and Loading model from Disk--------------
save_directory="my_model_dir"
tokenizer.save_pretrained(save_directory)
# ('my_model_dir/tokenizer_config.json',
#  'my_model_dir/special_tokens_map.json',
#  'my_model_dir/spiece.model',
#  'my_model_dir/added_tokens.json',
#  'my_model_dir/tokenizer.json')

model.save_pretrained(save_directory)
classifier = pipeline("sentiment-analysis", model=model, tokenizer=tokenizer,device=0)
res = classifier("I've been waiting for a HuggingFace course my whole life.")
print(res)
```
