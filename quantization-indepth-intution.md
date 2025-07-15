# Quantization Indepth Intution

1. Full Precision / Half Precission ⇒ How the data is stored in memory ⇒ Weights and parameters in LLM
2. Calibration ⇒ Also known as model quantization&#x20;
3. Modes of Quantization
   1. Post training
   2. Quantization aware

**Quantization:**

* Conversion from Higher memory format to a lower memory format
* When we train NN we have weights which are in the form of matrix
* In this every value is stored in the memory in 32bytes ⇒ denoted as FP32 ⇒ Full precision or single precision ⇒ this weight is like floating point like 7.32 and is stored in floating point 32
* In big LLM the number of parameters will be very high
* We cannot fine tune such a model as RAM / memory will be limited
*   We can also take a cloud instance and then try there ⇒ But here cost will be high

    <figure><img src=".gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>
* We can convert this 32 bit into 16 bit ⇒ This is known as Half precision ⇒ So inferencing will be possible here
* This is known as Quantization
* Its important, coz it will increase inference speed
* if we convert 32bit to 8 bit then calculation will be faster
* We do this in CV/NLP models also
* After we can deploy it on any mobile or edge device
* Once we Quantize, after that we can do fine tuning also
  * But due to quantization there will be some loss of accuracy
