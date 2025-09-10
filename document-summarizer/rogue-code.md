# ROGUE - Code

* &#x20;

```python
from rouge_score import rouge_scorer
import numpy as np

# Suppose we have 10 docs
candidates = [
    "The contract has penalties for missing deadlines.",
    "The system must be upgraded to improve performance.",
    # ... up to 10 candidate summaries
]

references = [
    "There are penalties when the deadline is missed.",
    "Upgrading the system will boost performance.",
    # ... up to 10 reference summaries
]

scorer = rouge_scorer.RougeScorer(['rouge1', 'rouge2', 'rougeL'], use_stemmer=True)

scores_all = []
for cand, ref in zip(candidates, references):
    score = scorer.score(ref, cand)
    scores_all.append(score)

# Aggregate over all documents
avg_scores = {}
for metric in scores_all[0].keys():
    avg_scores[metric] = {
        "precision": np.mean([s[metric].precision for s in scores_all]),
        "recall": np.mean([s[metric].recall for s in scores_all]),
        "f1": np.mean([s[metric].fmeasure for s in scores_all])
    }

print(avg_scores)

```
