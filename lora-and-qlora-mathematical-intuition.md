# LORA and QLORA mathematical intuition

Full Parameter fine tuning:

* Base model has been trained with huge amount of data
* Suppose we do fine tuning all the weight of the base model ⇒ This is known as full parameter fine tuning
* We can also perform further domain specific fine tuning
* We can also perform further specific task fine tuning
* Challenges:
  * Update all the model weights
  * Hardware resource constraint
*   To overcome this we will be using LORA and QLORA

    <figure><img src=".gitbook/assets/{17EAAC36-2437-4A36-85BA-7F582D455291}.png" alt=""><figcaption></figcaption></figure>

LoRA:

* Research paper ⇒ Low Rank Adapton of large language models
* Used in fine tuning
* Instead of updating weight, it will track the changes
* Pre trained + Lora tracked weights = Fine tuned weights
* Resource constraint wont happen, as matrix decomposition will be used here
*

    <figure><img src=".gitbook/assets/{4D898985-B8C7-4FF7-939B-A6A394DA32A0}.png" alt=""><figcaption></figcaption></figure>
*

    <figure><img src=".gitbook/assets/{0036C50A-3BA1-4248-BB69-D08CA2637155}.png" alt=""><figcaption></figcaption></figure>
* There will be loss in precision, but number of weights will be reduced
* Rank ==== To be google and check
*

    <figure><img src=".gitbook/assets/{E9ECBFC4-45D1-44CA-9367-108E0AAFDF63}.png" alt=""><figcaption></figcaption></figure>
*

    <figure><img src=".gitbook/assets/{E38DF4C7-669F-4050-BEDD-F5C4BA649A2E}.png" alt=""><figcaption></figcaption></figure>
* If the model wants to learn complex things, then use high rank

**QLORA:**

* Quantized LORA
* In this the weight will be converted to lower bits
