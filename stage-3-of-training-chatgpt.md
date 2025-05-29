# 🟢 Stage 3 of Training ChatGPT

**Reinforcement learned human feedback:**

* <mark style="color:purple;background-color:purple;">**Align their outputs with human preferences and values.**</mark>
* Agent interacts with its environment and learns to make decisions by getting rewarded or punished
* A reward function is used to measure success in RL
* Reward function is established using
  * Responses are ranked
  * The reward model gives a high score to ChatGPT when its response is really good compared to the other responses.
* The reward model spits out scores for each response. The bigger the score, the more likely the model thinks that response is preferred.
* The whole point of this model is to make the cross-entropy loss as low as possible during training so that it’ll be better at predicting stuff it hasn’t seen before.
* The reward model is like a binary classifier that uses standard cross-entropy as its loss function.
*

    <figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
