# Bits and Byte Config

* &#x20;Load in 4 bit
* nf4 means normal float 4 bit
* If model is trained with fp32 we are loading it here with fp4
* Here loading + Computations are happening
* During loading it will be fp4, but for computation it will be fp16
* Storing in memory means VRAM
* Computation means running the model with fp16
*
*

    <figure><img src=".gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>
