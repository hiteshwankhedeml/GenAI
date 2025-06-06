# Tokenizer

* Each token has id
* Sentence ⇒ Tokens ⇒ ID
* Tokenizer also add SOS and EOS tokens - Start of sentence, End of sentence
* This will be passed to Model for embedding, attention etc
* Every model has its own tokenizer
* <mark style="color:purple;background-color:purple;">**101 token means SOS and 102 means EOS**</mark>
* <mark style="color:purple;background-color:purple;">**token\_type\_id ⇒ for 1st sentence it will be 0, for 2nd it will be 1 and so on**</mark>
* Sentence ⇒ Token ⇒ IDs ⇒ Embedding ⇒ Position encoding ⇒ Self attention
* <mark style="color:purple;background-color:purple;">**If the sentence length is mis-match then tokenizer adds padding also**</mark>
* <mark style="color:purple;background-color:purple;">**Steps:**</mark>
  * <mark style="color:purple;background-color:purple;">**tokenizer.tokenize ⇒ to see the tokens of the sentence**</mark>
  * <mark style="color:purple;background-color:purple;">**tokenizer.convert\_tokens\_to\_ids ⇒ to convert tokens to ids**</mark>
  * <mark style="color:purple;background-color:purple;">**tokenizer.decode(ids) ⇒ This will give decode token ids to give the string back**</mark>
  * <mark style="color:purple;background-color:purple;">**tokenizer(sentences) ⇒ this will given token ids, token\_type\_ids, attention\_masks**</mark>

```python
from transformers import BertTokenizerFast
tokenizer = BertTokenizerFast.from_pretrained("bert-base-uncased")

sequence1 = "Using a Transformer network is simple"
sequence2 = "It is very powerful"

res = tokenizer(sequence1,sequence2)
print(res)

# {'input_ids': [101, 2478, 1037, 10938, 2121, 2897, 2003, 3722, 102, 
# 2009, 2003, 2200, 3928, 102], 'token_type_ids': [0, 0, 0, 0, 0, 0, 0, 0, 0,
# 1, 1, 1, 1, 1], 'attention_mask': [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]}

sequence = "Using a Transformer network is simple"
tokens = tokenizer.tokenize(sequence)
print(tokens)
# ['using', 'a', 'transform', '##er', 'network', 'is', 'simple']

ids = tokenizer.convert_tokens_to_ids(tokens)
print(ids)
# [2478, 1037, 10938, 2121, 2897, 2003, 3722]

decoded_string = tokenizer.decode(ids)
print(decoded_string)
# using a transformer network is simple
```
