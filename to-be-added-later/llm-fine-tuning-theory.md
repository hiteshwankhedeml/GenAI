# LLM Fine tuning - Theory

* Finetuning of LLM model
* LLMs have billions of parameters
* GPT4 has trillion parameter
* So we cannot fine tune it like LM models
* BERT was a LM model, we 1st downloaded pretrained model and then fine tuned it

**LLM Fine tuning Stages:**

1. Unsupervised Pretraining
2. Supervised fine tuning ⇒ Implement PEFT
3. RLFH (DPO, PPO)



**PEFT:**

* Parameter efficient fine tuning
* We cannot directly SFT on LLM directly
* Take subset of trainable parameters
* In transformers, we have attention layers which has Q matrix, NN ⇒ this also has weights and biases
* Different techniques like LoRA
* Quantization + LoRA ⇒ QLoRA
* Quantization means reduce the precission of the parameter



**LoRA:**

* Low Rank Adaption
* Using LoRa we take subset of parameters



**LLM Quantization techniques:**

* GGML
* GGUF
* GPTQ ⇒ GPT Quantization
