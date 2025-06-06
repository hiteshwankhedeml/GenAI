# Quantization

* 2.87 ⇒ float 32 ⇒ So this will occupy 4 bytes
* Lets say this is single weight
* So for 175B weights, it will take 175B \* 4 Bytes == This much memory will be taken by GPT3 model
* **So in Quantization, we reduce the precision**
* If we reduce from float32 to Int8, then it will reduce from 4 byte to 1 byte
* Quantization comes with loss of information, if we reduce too much precision
