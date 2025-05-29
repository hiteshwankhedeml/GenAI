# 🟢 BERT Blog - OpenAI Transformer

* <mark style="color:purple;background-color:purple;">**It turns out we don’t need an entire Transformer to adopt transfer learning and a fine-tunable language model for NLP tasks.**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**We can do with just the decoder of the transformer. The decoder is a good choice because it’s a natural choice for language modeling (predicting the next word) since it’s built to mask future tokens – a valuable feature when it’s generating a translation word by word.**</mark>
* <mark style="color:purple;background-color:purple;">**We can proceed to train the model on the same language modeling task: predict the next word using massive (unlabeled) datasets**</mark>
*

    <figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
