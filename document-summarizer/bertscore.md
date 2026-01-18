---
hidden: true
---

# ✈️ BERTScore

BERTScore evaluates summaries by computing **semantic similarity** between candidate and reference using **contextual embeddings** from transformer models (e.g., BERT, RoBERTa).



* **Low similarity:** 0.2–0.5 → candidate only partially captures meaning
* **Moderate similarity:** 0.5–0.7 → candidate captures most key points
* **High similarity:** 0.7–0.9 → candidate closely matches reference meaning
* **Excellent:** 0.9+ → almost identical meaning



**How it works (stepwise):**

1. Tokenize both **candidate** and **reference summaries**.
2. Convert tokens into **BERT embeddings**.
3. Compute **cosine similarity** between each candidate token and all reference tokens.
4. For each candidate token, take **max similarity** → gives **precision**.
5. For each reference token, take **max similarity** → gives **recall**.
6. Compute **F1 score** = 2 × (Precision × Recall) / (Precision + Recall).



**Example:**

**Reference summary:**

```
The contract has penalties for missing deadlines.
```

**Candidate summary:**

```
Severe fines apply if the contract deadline is missed.
```

**Step 1:** Tokenize

* Reference tokens: `["the", "contract", "has", "penalties", "for", "missing", "deadlines"]`
* Candidate tokens: `["severe", "fines", "apply", "if", "the", "contract", "deadline", "is", "missed"]`

**Step 2:** Compute token cosine similarity (hypothetical values for illustration)

| Candidate Token | Max similarity with reference tokens |
| --------------- | ------------------------------------ |
| severe          | 0.6 (with "penalties")               |
| fines           | 0.9 (with "penalties")               |
| apply           | 0.5 (with "has")                     |
| if              | 0.4 (with "for")                     |
| the             | 1.0 (with "the")                     |
| contract        | 1.0 (with "contract")                |
| deadline        | 0.95 (with "deadlines")              |
| is              | 0.5 (with "has")                     |
| missed          | 0.9 (with "missing")                 |

**Step 3:** Compute Precision and Recall

* **Precision** = avg(max similarities for candidate tokens)

(0.6+0.9+0.5+0.4+1.0+1.0+0.95+0.5+0.9)/9≈0.71(0.6 + 0.9 + 0.5 + 0.4 + 1.0 + 1.0 + 0.95 + 0.5 + 0.9) / 9 ≈ 0.71

* **Recall** = avg(max similarities for reference tokens)

(1.0+1.0+0.5+0.9+0.4+0.9+0.95)/7≈0.78(1.0 + 1.0 + 0.5 + 0.9 + 0.4 + 0.9 + 0.95) / 7 ≈ 0.78

* **F1 score** = 2 × (Precision × Recall) / (Precision + Recall)

F1≈2×(0.71×0.78)/(0.71+0.78)≈0.745F1 ≈ 2 × (0.71 × 0.78) / (0.71 + 0.78) ≈ 0.745

*   **Interpretation:**

    * High F1 (\~0.75) even though **words differ**, because meaning is captured.


*   **Why BERTScore is good for abstractive summaries:**

    * Captures **semantic similarity**, not just exact words.
    * Handles **paraphrasing** and **neural model outputs** well.
    * More **robust for domain-specific text**.


* **Limitations / Cons:**
  * Computationally expensive vs ROUGE.
  * Sensitive to **pretrained model choice**.
  * Does not fully account for **sentence order/sequence**.
