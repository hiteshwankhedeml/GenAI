---
hidden: true
---

# ✈️ When to use which LLM?

| **Use-case**                                      | **Best LLM Type**                        | **Reason**                            | **Examples (Open-Source)**                 |
| ------------------------------------------------- | ---------------------------------------- | ------------------------------------- | ------------------------------------------ |
| **Answer Scoring / Classification**               | Small Instruct Models (7B–8B)            | Stable, deterministic, cheap, fast    | Llama-3 8B, Mistral 7B, Qwen2 7B           |
| **Text Generation / Creative Output**             | Medium–Large Models                      | Better reasoning + creativity         | Llama-3 14B, Mixtral 8×7B, Qwen2 14B       |
| **Structured Extraction (NER, fields, criteria)** | Small Encoder-style Instruct Models      | Precise extraction, low hallucination | Llama-3 8B, Mistral 7B                     |
| **RAG (Search + Answering)**                      | Models good at retrieval + summarization | Better grounding, follows documents   | Mistral 7B, Qwen2 7B, Llama-3 8B           |
| **Two-way Interview Bots (Low Latency)**          | 7B–8B models                             | Fast responses, fits single GPU       | Llama-3 8B, Mistral 7B                     |
| **Long Answers / Big Context**                    | Long-context models                      | Can handle long paragraphs            | Llama-3 8B 128k, Qwen2 Long, Mistral Large |
| **Multilingual / Indian Languages**               | Multilingual LLMs                        | Better Hindi/Indian-accent text       | Qwen2 7B, Aya 23B, Indic models            |
| **High-accuracy Reasoning**                       | Larger models                            | Strong logic, complex reasoning       | Llama-3 70B, Qwen2 72B, Mixtral 8×22B      |
| **Low GPU / On-Prem Budget**                      | Efficient smaller models                 | Run on 1 GPU, low cost                | Phi-3 Mini, Llama-3 8B, Mistral 7B         |
| **Privacy-sensitive workloads**                   | Fully open-source on-prem models         | No data leaves company                | Llama-3 family, Qwen2, Mistral             |
