---
hidden: true
---

# ✈️ BERTScore - Code

* It uses bert-base-uncased by default
* It uses transformer api for getting embeddings
* We can also use other models, by passing model\_id

```python
# Install bert-score if not already installed
# pip install bert-score

from bert_score import score

# Example: 10 candidate summaries and their reference summaries
candidates = [
    "Severe fines apply if the contract deadline is missed.",
    "The system needs upgrading to improve performance.",
    "Employees must submit reports on time.",
    "Data security protocols must be followed.",
    "Payments are processed within 30 days.",
    "The agreement has strict confidentiality clauses.",
    "Late submissions incur penalties.",
    "Software updates are mandatory for compliance.",
    "All meetings must be documented.",
    "Customer complaints should be addressed promptly."
]

references = [
    "The contract has penalties for missing deadlines.",
    "System upgrades are necessary for better performance.",
    "Reports must be submitted by employees on schedule.",
    "Security protocols for data must be observed.",
    "Payments should be made within thirty days.",
    "The contract enforces confidentiality strictly.",
    "Penalties apply for late submissions.",
    "Compliance requires mandatory software updates.",
    "Documentation of all meetings is required.",
    "Complaints from customers must be resolved quickly."
]

# Compute BERTScore
P, R, F1 = score(candidates, references, lang="en", verbose=True)

# Average scores across all documents
print(f"Average Precision: {P.mean().item():.3f}")
print(f"Average Recall: {R.mean().item():.3f}")
print(f"Average F1: {F1.mean().item():.3f}")
```

***

#### **Explanation**

* `score()` computes **Precision, Recall, F1** for each candidate-reference pair.
* `lang="en"` uses an English pretrained model (BERT base by default).
* `P.mean()`, `R.mean()`, `F1.mean()` gives **average BERTScore across all 10 documents**.
* Works **out-of-the-box** for multiple documents without aligning sentences manually.
