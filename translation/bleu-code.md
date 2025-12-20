# 🟢 BLEU - Code

```python
from nltk.translate.bleu_score import corpus_bleu

references = [
    ["the cat is on the mat".split()],
    ["there is a cat on the mat".split()]
]
candidates = [
    "the cat is on mat".split(),
    "there is cat on mat".split()
]

bleu = corpus_bleu(references, candidates)
print(bleu)
```
