# Llama

* [https://huggingface.co/meta-llama](https://huggingface.co/meta-llama)
* Research paper in <mark style="color:purple;background-color:purple;">**2023**</mark>
* <mark style="color:purple;background-color:purple;">**Decoder based architecture**</mark>
* Parameters from 7 billions to 70 billions
* Latest model is having 401B parameters
* <mark style="color:purple;background-color:purple;">**Optimized for dialog use case**</mark>
* <mark style="color:purple;background-color:purple;">**Developed by Meta as competitor of OpenAI**</mark>
* <mark style="color:purple;background-color:purple;">**Open source model**</mark>
* New positional encoding introduced ⇒ ROPE
* Rotary positional embedding ⇒ this is for capturing longer sentences
* In supervised fine tuning, they did dialog specific training ( Question and Answer form)
* Developed by Meta (Facebook) as an open-weight LLM family.
* Available in multiple sizes: 7B, 13B, 65B parameters.
* Architecture: decoder-only Transformer similar to GPT.
* Trained on a mix of publicly available datasets with careful data curation.
* Strong performance on various benchmarks despite smaller size compared to models like GPT-3.
* Supports up to **4K token context length** by default.
* Uses standard Multi-Head Attention (MHA), not grouped-query attention.
* Uses **RoPE (Rotary Positional Embeddings)** for positional encoding, which rotates query and key vectors to better capture token positions and generalize to longer sequences.
* Openly available weights encourage research and fine-tuning.
* Often fine-tuned for instruction-following tasks (e.g., Alpaca, Vicuna).
* Popular baseline for many research and production use cases.
* Limitations: relatively smaller context length and higher inference cost than newer efficient models like Mistral.
