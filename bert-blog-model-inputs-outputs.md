# 🟢 BERT Blog - Model Inputs / Outputs

*

    <figure><img src=".gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>
* <mark style="color:purple;background-color:purple;">**The first input token is supplied with a special \[CLS] token. CLS here stands for Classification.**</mark>
* <mark style="color:purple;background-color:purple;">**Each position outputs a vector of size**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark>_<mark style="color:purple;background-color:purple;">**hidden\_size**</mark>_<mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**(768 in BERT Base). For the sentence classification example we’ve looked at above, we focus on the output of only the first position (that we passed the special \[CLS] token to).**</mark>
* <mark style="color:purple;background-color:purple;">**That vector can now be used as the input for a classifier of our choosing.**</mark> The paper achieves great results by just using a single-layer neural network as the classifier.
*

    <figure><img src=".gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>
* If you have more labels (for example if you’re an email service that tags emails with “spam”, “not spam”, “social”, and “promotion”), you just tweak the classifier network to have more output neurons that then pass through softmax\\
