# BERT

* Bidirectional Encoder Representation from Transformer
* Encoder based architecture
* Bidirectional means it is going to process the data in both the directions to self attention layer
* We have positional encoding also
* Trained on massive wikipedia data
* Layer Normalization and Feed Forward
* 2 models
  * Small model having 12 layers ⇒ Hidden layer size in Feed forward NN ⇒ 768 ⇒ 110M parameters
  * Large model having 24 layers ⇒ Hidden layer size in Feed forward NN ⇒ 1024 ⇒ 340M parameters
*

    <figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
* BERT was mainly trained for text classification
* It was used for summarization also
* Base model can be fine tuned for any task
* In BERT 2 stages were involved
  * Unsupervised pre training
    * Masked Language Modelling
    * Next sentence prediction
  * Supervised fine tuning
* Huge data from books, wikipedia, reddit, facebook etc.
