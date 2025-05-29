# 🟢 BERT Blog - ELMo's secret

* <mark style="color:purple;background-color:purple;">**ELMo gained its language understanding from being trained to predict the next word in a sequence of words - a task called Language Modeling.**</mark>
*

    <figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
* ELMo actually goes a step further and trains a bi-directional LSTM – so that its language model doesn’t only have a sense of the next word, but also the previous word.

**Step 1:**

*

    <figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

**Step 2:**

*   <mark style="color:purple;background-color:purple;">**ELMo comes up with the contextualized embedding through grouping together the hidden states (and initial embedding) in a certain way (concatenation followed by weighted summation).**</mark>

    <figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>
