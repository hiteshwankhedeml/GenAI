# BERT - Code

* &#x20;For fine tuning, we need atleast 1000 to 2000 rows
* It depends on model also, for gpt we can train using 100 or 200 rows also
* BERT is not trained on that huge data
* Disable WANDM\_DISABLED other it will update logs of weights and biases to their website and it required key, so we disable it
* Chunking is also like POS, but it gives more details

```python
import datasets
import numpy as np
from transformers import BertTokenizerFast
from transformers import DataCollatorForTokenClassification
from transformers import AutoModelForTokenClassification

import os
os.environ["WANDB_DISABLED"] = "true"

conll2003 = datasets.load_dataset("conll2003")
#CoNLL-2003 ek NER (Named Entity Recognition) dataset hai, jo 2003 ke 
#CoNLL (Conference on Computational Natural Language Learning) me introduce
# kiya gaya tha. Yeh dataset news articles ka collection hai aur 4 entity 
#types detect karne ke liye use hota hai:
#PER → Person (e.g., "Elon Musk")
#LOC → Location (e.g., "India", "New York")
#ORG → Organization (e.g., "Google", "NASA")
#MISC → Miscellaneous (e.g., "Olympics", "iPhone")

conll2003
#DatasetDict({
#    train: Dataset({
#        features: ['id', 'tokens', 'pos_tags', 'chunk_tags', 'ner_tags'],
#        num_rows: 14041
#    })
#    validation: Dataset({
#        features: ['id', 'tokens', 'pos_tags', 'chunk_tags', 'ner_tags'],
#        num_rows: 3250
#    })
#    test: Dataset({
#        features: ['id', 'tokens', 'pos_tags', 'chunk_tags', 'ner_tags'],
#        num_rows: 3453
#    })
#})

conll2003["train"]. features['ner_tags']
# Sequence(feature=ClassLabel(names=['O', 'B-PER', 'I-PER', 'B-ORG',
# 'I-ORG', 'B-LOC', 'I-LOC', 'B-MISC', 'I-MISC'], id=None), length=-1, id=None)

conll2003["train"][0]
#{'id': '0',
# 'tokens': ['EU',
#  'rejects',
#  'German',
#  'call',
#  'to',
#  'boycott',
#  'British',
#  'lamb',
#  '.'],
# 'pos_tags': [22, 42, 16, 21, 35, 37, 16, 21, 7],
# 'chunk_tags': [11, 21, 11, 12, 21, 22, 11, 12, 0],
# 'ner_tags': [3, 0, 7, 0, 0, 0, 7, 0, 0]}

example_text=conll2003['train'][0]

example_text["tokens"]
# ['EU', 'rejects', 'German', 'call', 'to', 'boycott', 'British', 'lamb', '.']

# We can use the pretrained model which is trained on internet data for inferencing to 
# check the results
# We can fine tune this model
from transformers import pipeline

model_name = "bert-base-uncased"

nlp_pipeline=pipeline("ner",model=model_name,tokenizer=tokenizer)

example="Sunny is Data Scientist and Generative AI Engineer"
nlp_pipeline(example)
# [{'entity': 'B-PER',
#   'score': 0.9791179,
#   'index': 1,
#   'word': 'sunny',
#   'start': 0,
#   'end': 5}]




```
