---
hidden: true
---

# ✈️ Difference between simple AI assistant and RAG

* We train model on lots of data ⇒ Pretrain ⇒ SFT ⇒ RLFH ⇒ Final LLM
* To this final LLM we are passing prompt and we get output
*

```
<figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
```

**Problems with LLM:**

* Hallucinations:
  * Output might be wrong
  * Output might be misinformation
* We might have taken internet data but for some specific domain, we might have not trained it
* So for such scenarios there are 2 solutions
  * Retrain the model
  * Use RAG
    * Provide more context to the LLM using few shot learning
