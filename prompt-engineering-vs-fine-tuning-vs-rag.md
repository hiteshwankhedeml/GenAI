# 🟢 Prompt Engineering vs Fine Tuning vs RAG

<figure><img src=".gitbook/assets/{027927BE-EEE1-4BAB-8A8B-B3B41DCD24B7}.png" alt=""><figcaption></figcaption></figure>

**Prompt Engineering:**

* Prompt should be specific instruction, structured with clear context
* Pros:
  * <mark style="color:purple;background-color:purple;">**No technical expertise needed**</mark>
  * Instant results
  * No training required
  * Works with any LLM
* Cons:
  * <mark style="color:purple;background-color:purple;">**Limited by models base knowledge**</mark>
  * Inconsistent results
  * Every model has token limit restriction
  * Cant add new knowledge
* Best for:
  * Small scale application
  * Generic purpose tasks
  * Quick prototyping

**Fine Tuning:**

* For company <mark style="color:purple;background-color:purple;">**specific use case**</mark>
* We will get the company specific training data
* <mark style="color:purple;background-color:purple;">**Weights will be modified**</mark>
* Train model on company's data
* Pros:
  * Deeply specialized knowledge
  * Consistent behavior
  * <mark style="color:purple;background-color:purple;">**No prompt engineering needed**</mark>
  * <mark style="color:purple;background-color:purple;">**Better for specific domain**</mark>
* Cons:
  * <mark style="color:purple;background-color:purple;">**Expensive as GPU will be needed**</mark>
  * Require ML expertise
  * <mark style="color:purple;background-color:purple;">**Regular retraining is required for updates**</mark>

Best for:

* Specific style
* High volumes of data
* When accuracy is critical



**RAG:**

* Store documents in vectorDB
* Retrieve relevant docs for each query
* LLM generate answer from context
* **Pros:**
  * <mark style="color:purple;background-color:purple;">**Always up to date info**</mark>
  * <mark style="color:purple;background-color:purple;">**No training required**</mark>
  * <mark style="color:purple;background-color:purple;">**Cost effective**</mark>
  * Accuracy is high&#x20;
  * Can also handle private / proprietary data
* **Cons:**
  * Infrastructure setup needed
  * Retrieval quality affects answers
  * <mark style="color:purple;background-color:purple;">**Context window limitation**</mark>
*



*

    <figure><img src=".gitbook/assets/{53D0812C-0BDD-4083-9C6A-CF383C81C0B4}.png" alt=""><figcaption></figcaption></figure>
