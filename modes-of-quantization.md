# Modes of Quantization

* **Post training quantization:**
  * We have a pre-trained model
  * So we perform calibration
  * We take weights data and then convert into quantized model
  * We can then use this model for any use case
  * There will be loss of accuracy
* **Quantization aware:**
  * Also call QAT
  * We will take trained model ⇒ Quantization ⇒ Fine tuning
  * For fine tuning we will be taking new training data
  * And then on this model we will be doing quantization
*

    <figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>
