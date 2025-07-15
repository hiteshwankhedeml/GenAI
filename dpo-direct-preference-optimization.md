# DPO - Direct Preference Optimization

* Alternative of RLHF - Which was used by OpenAI to train ChatGPT
* For training LLM - We 1st do unsupervised pre-training on Raw training data, followed by SFT and then RLHF
* OpenAI was using RL using PPO (But its not used now)
* Not a RL based learning
* &#x20;It uses a supervised loss function
* DPO directly for preference based learning
* Lets say we ask How to invest money to SFT model ⇒ It gives ethical and unethical answer ⇒ Based on this preference data, we again train the data
* In DPO, we will give preference data to SFT model and then do training again, but here loss function will be different, it will be a supervised loss function
*

    <figure><img src=".gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>
