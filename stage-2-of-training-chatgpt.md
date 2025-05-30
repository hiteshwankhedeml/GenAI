# 🟢 Stage 2 of Training ChatGPT

* When we download a model its usually pretrained only and not fine tuned for any specific task
* However, some models on Hugging Face are **already fine-tuned** and tagged accordingly (e.g., `bert-base-uncased-finetuned-sst2` for sentiment analysis). You can look at the model card or description to check if it’s pretrained only or fine-tuned.

**Supervised Fine Tuning (SFT)**

* <mark style="color:purple;background-color:purple;">**During this stage, the model gets trained on specific tasks that are relevant to what the user is looking for, like conversational chat**</mark>
* Create training corpus
* Fine tune the model using this corpus and update the parameters using SGD to learn task
*

    <figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>
