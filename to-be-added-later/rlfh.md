# RLFH

* &#x20;Training framework
* By OpenAI
* It used PPO
* We have raw data ⇒ On top of this we have trained a unsupervised model
* On top if it we did SFT (PEFT + Quantization)
* After that we will be doing RLFH
* After doing SFT, our model is ready for conversation
* But it might give any output, so we want to regulate the model as we don't want any biased, toxic, unethical response
* So the aim of RLFH was to generate controlled and ethical response&#x20;
* Generate human preference response
*

    <figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
* &#x20;To SFT model we are asking - How to make money
* It might answer rob bank, take loan, hard work
* Out of this not all are ethical answer
* So here as a human we have given preferences like illegal, risky good etc
* On top of this we will again train our model
* OpenAI has used RL + PPO
* They had preference data, it was passed to model(Agent)
* In DL we do FP ⇒ Calculate Loss ⇒ BP using optimizer
* So here PPO is optimizer
*   This was done to generate safe and helpful responses

    <figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
