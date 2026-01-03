---
hidden: true
---

# ✈️ MultiModal RAG

* MultiModal ⇒ Text + Image
* MultiModal LLM ⇒ Trained on text + images
* OpenAI ⇒ gpt 4.1
* Google gemini flash 2.5

**Steps:**

* PDF document&#x20;
  * Extract text and images
  * Text ⇒ Chunk ⇒ Embedding
  * Images ⇒ Embedding
  * Store the images as base64
* Query ⇒ Using clip to convert into embedding ⇒ vector search
* Get top k documents (text + image)
* Before give it to LLM convert into specific format
* Finally we will be getting multimodal answer
* CLIP can do embeddings for text and images ⇒ provided by OpenAI (Opensource)
*
