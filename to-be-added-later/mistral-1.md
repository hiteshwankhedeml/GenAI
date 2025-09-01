# Mistral

* <mark style="color:purple;background-color:purple;">**Few models are open source, rest are paid**</mark>
* [https://mistral.i/](https://mistral.ai/)
* [https://huggingface.co/mistralai](https://huggingface.co/mistralai)
* They are using Grouped Query Attention and sliding window attention
* Research paper:&#x20;
* French based company
* Decoder based model
* Mistral is a **7B parameter open-weight LLM** released by **Mistral AI**
* Architecture: **Decoder-only Transformer**, similar to GPT
* Uses **Grouped-Query Attention (GQA)** for **faster inference** and **lower memory usage**
* Supports **8K token context length**
* Supports **sliding window attention**
  * Enables processing long sequences in **chunks**
  * Useful for **streaming** or tasks needing **overlapping context**
  * Reduces memory load compared to full-context attention
* There are two versions:
  * **Base Mistral** → only **pretrained** (next-token prediction)
  * **Mistral-Instruct** → **instruction-tuned** version of base Mistral
* **Base Mistral**:
  * Not trained to follow instructions
  * Performs poorly on prompts like "Summarize" or "Translate"
  * Needs further fine-tuning (e.g., SFT, DPO, RLHF) for task-following
* **Mistral-Instruct**:
  * Fine-tuned using instruction datasets
  * Can handle tasks like Q\&A, summarization, translation, etc.
* Despite being 7B, **outperforms LLaMA 2 13B** on benchmarks like MMLU, ARC
* Trained on a **mix of public and licensed data** (sources undisclosed)
* Highly **efficient for deployment** on both CPU and GPU
* Good candidate for **custom fine-tuning** because:
  * Small size → fits on limited hardware
  * Efficient architecture → lower compute/memory use
  * Strong base → small tuning gives large gains
  * Compatible with LoRA, QLoRA, PEFT
  * Long context window → suitable for document-level tasks
  * Available on Hugging Face with tooling support
* Common interview questions:
  * Difference between **GQA and MHA**
  * Why Mistral 7B outperforms larger models
  * Role of instruction tuning
  * Use cases for base vs instruct models
  * Purpose of **sliding window attention**
  * Why it’s suitable for **custom fine-tuning**

Let me know if you want the same for **Mixtral (MoE version)** or **Gemma, Yi, etc.**
