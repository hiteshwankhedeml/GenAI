# Types of Data Collator

#### <mark style="color:purple;background-color:purple;">1.</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">**DataCollatorWithPadding**</mark>

* **Use:** General padding of input sequences for classification or token tasks (no special label handling).
* **Example:** Batch of tokenized sentences:

| Input (token ids)             |
| ----------------------------- |
| \[101, 2002, 3264, 102]       |
| \[101, 2016, 3404, 4735, 102] |

* **Without padding:** Lengths 4 and 5
* **With DataCollatorWithPadding:** P<mark style="color:purple;background-color:purple;">**ads shorter sequence with**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`[0]`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**(pad token id) and its attention mask with be 0**</mark>:

| Padded input\_ids             | attention\_mask  |
| ----------------------------- | ---------------- |
| \[101, 2002, 3264, 102, 0]    | \[1, 1, 1, 1, 0] |
| \[101, 2016, 3404, 4735, 102] | \[1, 1, 1, 1, 1] |

***

#### <mark style="color:purple;background-color:purple;">2.</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">**DataCollatorForTokenClassification**</mark>

* **Use:** Token classification (e.g., NER) with labels per token.
* **Example:** Same inputs with labels per token:

| input\_ids                    | labels                 |
| ----------------------------- | ---------------------- |
| \[101, 2002, 3264, 102]       | \[-100, 0, 1, -100]    |
| \[101, 2016, 3404, 4735, 102] | \[-100, 0, 1, 2, -100] |

* **After padding:**

| Padded input\_ids             | Padded labels             | attention\_mask  |
| ----------------------------- | ------------------------- | ---------------- |
| \[101, 2002, 3264, 102, 0]    | \[-100, 0, 1, -100, -100] | \[1, 1, 1, 1, 0] |
| \[101, 2016, 3404, 4735, 102] | \[-100, 0, 1, 2, -100]    | \[1, 1, 1, 1, 1] |

* <mark style="color:purple;background-color:purple;">**Labels padded with**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`-100`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**(ignored in loss).**</mark>

***

#### <mark style="color:purple;background-color:purple;">3.</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">**DataCollatorForLanguageModeling**</mark>

* **Use:** MLM pretraining (like BERT).
* **Example input:**

| input\_ids              |
| ----------------------- |
| \[101, 2002, 3264, 102] |

* **What happens:** <mark style="color:purple;background-color:purple;">**Random tokens replaced with**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`[MASK]`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**token during batching**</mark>, e.g.:

| masked\_input\_ids     | labels (original tokens or -100 for unmasked) |
| ---------------------- | --------------------------------------------- |
| \[101, 2002, 103, 102] | \[-100, -100, 3264, -100]                     |

* `103` is the `[MASK]` token id.

***

#### 4. **DataCollatorForSeq2Seq**

* **Use:** Seq2Seq tasks (translation, summarization). Pads inputs and decoder targets separately.
* **Example:**

| input\_ids (source)           | labels (target)         |
| ----------------------------- | ----------------------- |
| \[101, 2002, 3264, 102]       | \[101, 2016, 3404, 102] |
| \[101, 2016, 3404, 4735, 102] | \[101, 3404, 102]       |

* **After padding:**

| padded\_input\_ids            | padded\_labels             | attention\_mask  | decoder\_attention\_mask |
| ----------------------------- | -------------------------- | ---------------- | ------------------------ |
| \[101, 2002, 3264, 102, 0]    | \[101, 2016, 3404, 102, 0] | \[1, 1, 1, 1, 0] | \[1, 1, 1, 1, 0]         |
| \[101, 2016, 3404, 4735, 102] | \[101, 3404, 102, 0, 0]    | \[1, 1, 1, 1, 1] | \[1, 1, 1, 0, 0]         |

***

#### 5. **DataCollatorForWholeWordMask**

* **Use:** MLM with whole word masking. Masks entire words (all sub-tokens) together.
* **Example:**

\| Tokens: \["lov", "##ing", "is", "fun"] |

* Instead of masking just "lov" or "##ing", masks both tokens together:

\| Masked Tokens: \["\[MASK]", "\[MASK]", "is", "fun"] |

***

If you want, I can share code snippets for any of these or more detailed examples!
