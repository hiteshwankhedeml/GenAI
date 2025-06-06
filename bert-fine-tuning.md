# BERT - Fine tuning

* <mark style="color:purple;background-color:purple;">**We also need to update the config.json as per the labels used for training**</mark>
* <mark style="color:purple;background-color:purple;">**We need to save the tokenizer and model**</mark>
* <mark style="color:purple;background-color:purple;">**We are usling tokenize\_and\_align\_labels to:**</mark>
  * <mark style="color:purple;background-color:purple;">**To handle word-to-subword splitting (like**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`"playing"`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**→**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`"play"`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**+**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`"##ing"`**</mark><mark style="color:purple;background-color:purple;">**) and align the original word-level NER labels to each resulting token correctly.**</mark>
  * <mark style="color:purple;background-color:purple;">**And also to assign -100 to padding**</mark>
* <mark style="color:purple;background-color:purple;">**Steps:**</mark>
  * <mark style="color:purple;background-color:purple;">Take the data set and tokenize and allign labels</mark>
  * <mark style="color:purple;background-color:purple;">Define training parameters</mark>
  * <mark style="color:purple;background-color:purple;">Define data collator</mark>
  * <mark style="color:purple;background-color:purple;">Define trainer API</mark>&#x20;
  * <mark style="color:purple;background-color:purple;">Train</mark>
  * <mark style="color:purple;background-color:purple;">Save json, tokenizer and model</mark>
  * <mark style="color:purple;background-color:purple;">For evaluation we dont use labels which had -100</mark>

```python
# Preparing data for fine tuning
# We can use the below function to align tokens and labelsn
def tokenize_and_align_labels(examples, label_all_tokens=True):

    #tokeinze ids
    tokenized_inputs = tokenizer(examples["tokens"], truncation=True, is_split_into_words=True)
    labels = []


    for i, label in enumerate(examples["ner_tags"]):
        word_ids = tokenized_inputs.word_ids(batch_index=i)
        # word_ids() => Return a list mapping the tokens
        # to their actual word in the initial sentence.
        # It Returns a list indicating the word corresponding to each token.

        previous_word_idx = None
        label_ids = []
        # Special tokens like `` and `<\s>` are originally mapped to None
        # We need to set the label to -100 so they are automatically ignored in the loss function.
        for word_idx in word_ids:
            if word_idx is None:
                # set –100 as the label for these special tokens
                label_ids.append(-100)

            # For the other tokens in a word, we set the label to either the current label or -100, depending on
            # the label_all_tokens flag.
            elif word_idx != previous_word_idx:
                # if current word_idx is != prev then its the most regular case
                # and add the corresponding token
                label_ids.append(label[word_idx])
            else:
                # to take care of sub-words which have the same word_idx
                # set -100 as well for them, but only if label_all_tokens == False
                label_ids.append(label[word_idx] if label_all_tokens else -100)
                # mask the subword representations after the first subword

            previous_word_idx = word_idx
        labels.append(label_ids)
    tokenized_inputs["labels"] = labels
    return tokenized_inputs

q=tokenize_and_align_labels(conll2003["train"][0:1])
for token, label in zip(tokenizer.convert_ids_to_tokens(q["input_ids"][0]),q["labels"][0]):
    print(f"{token:_<40} {label}")
# [CLS]___________________________________ -100
# eu______________________________________ 3
# rejects_________________________________ 0
# german__________________________________ 7
# call____________________________________ 0
# to______________________________________ 0
# boycott_________________________________ 0
# british_________________________________ 7
# lamb____________________________________ 0
# ._______________________________________ 0
# [SEP]___________________________________ -100
    
tokenized_datasets = conll2003.map(tokenize_and_align_labels, batched=True)
# THis we will be using for fine tuning

from transformers import TrainingArguments, Trainer
import evaluate
metric = evaluate.load("seqeval")

#these are hyperparameter
args=TrainingArguments(
    "test-ner",
    evaluation_strategy = "epoch",
    learning_rate=2e-5,
    per_device_train_batch_size=16,
    per_device_eval_batch_size=16,
    num_train_epochs=1,
    weight_decay=0.01,
    report_to="none"  # Disable wandb logging
)

data_collator=DataCollatorForTokenClassification(tokenizer)

def compute_metrics(eval_preds):
    pred_logits, labels = eval_preds

    pred_logits = np.argmax(pred_logits, axis=2)
    # the logits and the probabilities are in the same order,
    # so we don’t need to apply the softmax

    # We remove all the values where the label is -100
    predictions = [
        [label_list[eval_preds] for (eval_preds, l) in zip(prediction, label) if l != -100]
        for prediction, label in zip(pred_logits, labels)
    ]

    true_labels = [
      [label_list[l] for (eval_preds, l) in zip(prediction, label) if l != -100]
       for prediction, label in zip(pred_logits, labels)
   ]
    results = metric.compute(predictions=predictions, references=true_labels)

    return {
          "precision": results["overall_precision"],
          "recall": results["overall_recall"],
          "f1": results["overall_f1"],
          "accuracy": results["overall_accuracy"],
  }

trainer=Trainer(
   model,
   args,
   train_dataset=tokenized_datasets["train"],
   eval_dataset=tokenized_datasets["validation"],
   data_collator=data_collator,
   tokenizer=tokenizer,
   compute_metrics=compute_metrics
)

trainer.train()
# [878/878 03:46, Epoch 1/1]
# Epoch	Training Loss	Validation Loss	Precision	Recall	F1	Accuracy
# 1	0.234900	0.066412	0.905907	0.923034	0.914390	0.981000

model.save_pretrained("ner_model") 
tokenizer.save_pretrained("tokenizer")

import json

config=json.load(open("/content/ner_model/config.json"))

id2label = {str(i): label for i,label in enumerate(label_list)}
label2id = {label: str(i) for i,label in enumerate(label_list)}

config["id2label"] = id2label
config["label2id"] = label2id

json.dump(config,open("/content/ner_model/config.json","w"))
config=json.load(open("/content/ner_model/config.json"))

model_name="sunny199/NER-Model-Fine-Tuned"
tokenizer = BertTokenizerFast.from_pretrained("/content/tokenizer")
model_fine_tuned=AutoModelForTokenClassification.from_pretrained("/content/ner_model")

model_fine_tuned=AutoModelForTokenClassification.from_pretrained(model_name)
nlp_pipeline=pipeline("ner",model=model_fine_tuned,tokenizer=tokenizer)

example="Sunny is Data Scientist and Generative AI Engineer"
nlp_pipeline(example)
# [{'entity': 'B-PER',
#   'score': 0.9791179,
#   'index': 1,
#   'word': 'sunny',
#   'start': 0,
#   'end': 5}]


```

