# 🟢 Sentence Embedding - Training

<figure><img src="../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

* Input is **two sentences**
* <mark style="color:purple;background-color:purple;">**Both sentences go into the same transformer model**</mark>
* The transformer weights are **shared** for both sentences
* Inside the transformer:
* Each sentence is split into tokens
* Tokens are converted into vectors
* Self-attention mixes context across words
* Output of transformer:
* <mark style="color:purple;background-color:purple;">**One vector per word (token embeddings)**</mark>
* Pooling step:
* <mark style="color:purple;background-color:purple;">**All word vectors are combined**</mark>
* <mark style="color:purple;background-color:purple;">**Usually by average (mean pooling)**</mark>
* Result is **one vector per sentence**
* Similarity step:
* Sentence-1 vector and Sentence-2 vector are compared
* **Cosine similarity** is calculated
* Loss calculation:
* <mark style="color:purple;background-color:purple;">**Cosine similarity is compared with the label**</mark>
* <mark style="color:purple;background-color:purple;">**Difference becomes the loss**</mark>
* <mark style="color:purple;background-color:purple;">**Loss is backpropagated to update transformer weights**</mark>
* Key idea:
* Same model + same weights
* Only the **loss tells the model what is right or wrong**
* One-line mental picture:
* <mark style="color:purple;background-color:purple;">**Two sentences → same transformer → two vectors → similarity → loss**</mark>
