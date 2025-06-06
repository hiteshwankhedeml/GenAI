# BERT

* <mark style="color:purple;background-color:purple;">**Bidirectional Encoder Representation from Transformer**</mark>
* <mark style="color:purple;background-color:purple;">**Encoder based architecture**</mark>
* <mark style="color:purple;background-color:purple;">**Bidirectional means it is going to process the data in both the directions to self attention layer**</mark>
* We have positional encoding also
* Trained on massive wikipedia data
* Layer Normalization and Feed Forward
* 2 models
  * Small model having 12 layers ⇒ Hidden layer size in Feed forward NN ⇒ 768 ⇒ 110M parameters
  * Large model having 24 layers ⇒ Hidden layer size in Feed forward NN ⇒ 1024 ⇒ 340M parameters
*

    <figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
* BERT was mainly trained for text classification
* It was used for summarization also
* Base model can be fine tuned for any task
* <mark style="color:purple;background-color:purple;">**In BERT 2 stages were involved**</mark>
  * <mark style="color:purple;background-color:purple;">**Unsupervised pre training**</mark>
    * <mark style="color:purple;background-color:purple;">**Masked Language Modelling**</mark>
    * <mark style="color:purple;background-color:purple;">**Next sentence prediction**</mark>
  * <mark style="color:purple;background-color:purple;">**Supervised fine tuning**</mark>
* Huge data from books, wikipedia, reddit, facebook etc.
