# 🟢 BERT Blog - From Encoders to Decoders

* ELMo’s language model was bi-directional, but the openAI transformer only trains a forward language model.
* So BERT used encoders to solve this
* <mark style="color:purple;background-color:purple;">**BERT used masked language model**</mark>
*   <mark style="color:purple;background-color:purple;">**The pre-training process includes an additional task: Given two sentences (A and B), is B likely to be the sentence that follows A, or not?**</mark>

    <figure><img src=".gitbook/assets/image (16) (1).png" alt=""><figcaption></figcaption></figure>
*

```
<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
```
