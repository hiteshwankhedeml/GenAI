# 🟢 HuggingFace - Pipeline

* <mark style="color:purple;background-color:purple;">**When using pipeline, we just need to specify task and model**</mark>
* <mark style="color:purple;background-color:purple;">**It loads the model in memory**</mark>
* <mark style="color:purple;background-color:purple;">**Here also it's possible to get the model and tokenizer from pipeline, and then save it locally**</mark>
* distilbert-base-uncases-finetuned-sst-2-english
  * BERT based
  * uncases means its not case sensitve
  * sst2 - stanford sentiment treebank 2 - dataset

```python
!pip install transformers

from transformers import pipeline

#------------Classification----------------
# This will download the model 
#distilbert/distilbert-base-uncased-finetuned-sst-2-english
# This is BERT
classifier=pipeline("sentiment-analysis") 
classifier("i will learn AI throughout my entire life it is like a passion")
# [{'label': 'POSITIVE', 'score': 0.9987781643867493}]

#------------Text-Generation----------------
generation=pipeline("text-generation") # Model downloaded - openai-community/gpt2
generation("python is a simple language what is your thought")
#[{'generated_text': 'python is a simple language what is your thought
# process when writing your tests? What about the rest? Why is Visual
# Studio better than Python?\n\nWhy Python?\n\nPython is a programming language 
# and is easy to learn without any complicated programming concepts'}]


#------------Summarization----------------
summarizer=pipeline("summarization") # sshleifer/distilbart-cnn-12-6
text = '.............'
print(summarizer(text,max_length=10))
[{'summary_text': ' A large language model (LLM'}]


#------------Zero Shot Classification----------------
classifier=pipeline("zero-shot-classification") # facebook/bart-large-mnli
res = classifier(
    "This is a course about Python list comprehension",
    candidate_labels=["education", "politics", "business"],
)
print(res)
# {'sequence': 'This is a course about Python list comprehension',
# 'labels': ['education', 'business', 'politics'],
# 'scores': [0.9622023105621338, 0.026841651648283005, 0.010956088081002235]}


#------------Mention the model to be loaded----------------
bert_summarizer=pipeline("summarization",model="google-bert/bert-base-uncased")
print(bert_summarizer(text,max_new_tokens=20))
```
