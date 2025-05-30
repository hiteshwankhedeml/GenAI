# 🟢 Stage 1 of Training ChatGPT

**Stage 1: Generative Pre-Training:**

* <mark style="color:purple;background-color:purple;">**For language understanding**</mark>
* <mark style="color:purple;background-color:purple;">**In pre-training it does next word prediction( it doesn't do MLM — this is done in BERT)**</mark>
* Trained on a bunch of data from internet
* <mark style="color:purple;background-color:purple;">**Decoder only, so there will be no encoder, You pass the full text into the decoder only.**</mark>
* It predicts the next token at each position based only on previous tokens.
* The training uses teacher forcing: model sees "The cat sat on" → predicts `"the"` → then `"cat"` → then `"sat"` and so on, comparing with ground truth.
* Cross entropy loss between actual and predicted
*

    <figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
* Model is training to do a lot of things like language modeling, summarization, translation, and sentiment analysis. It’s not trained for a specific task but can handle a bunch of different ones
