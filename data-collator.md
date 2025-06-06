# Data Collator

* Data collator batches multiple samples together during training or evaluation.
* <mark style="color:purple;background-color:purple;">**It handles dynamic padding, ensuring all sequences in a batch have the same length by padding shorter ones.**</mark>
* Creates attention masks to indicate which tokens are real and which are padding.
* Does not perform tokenization or label alignment.
* Works on already tokenized inputs with aligned labels.
* Helps efficiently process batches with variable-length sequences.
* Used to prepare inputs in the format required by the model during training.
* For token classification tasks, `DataCollatorForTokenClassification` is commonly used.
* <mark style="color:purple;background-color:purple;">**It ensures labels are padded properly along with input IDs and attention masks.**</mark>
