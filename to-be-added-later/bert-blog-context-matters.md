# 🟢 BERT Blog - Context Matters

* Earlier vector used for represention used to be same irrespective of context in the previous embedding methods
* This was proposed by NLP researchers ([Peters et. al., 2017](https://arxiv.org/abs/1705.00108), [McCann et. al., 2017](https://arxiv.org/abs/1708.00107), and yet again [Peters et. al., 2018 in the ELMo paper](https://arxiv.org/pdf/1802.05365.pdf) )
* <mark style="color:purple;background-color:purple;">**Instead of using a fixed embedding for each word, ELMo looks at the entire sentence before assigning each word in it an embedding.**</mark>
*

    <figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
* The ELMo LSTM would be trained on a massive dataset in the language of our dataset, and then we can use it as a component in other models that need to handle language.
