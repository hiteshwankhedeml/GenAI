---
hidden: true
---

# ✈️ Scoring - Chunk Split

* Split answer into sentences
* Group sentences until chunk has 15–60 words
* If a sentence is > 60 words, break it into two chunks
* No chunk should be too small (< 15 words)
* No chunk should be too large (> 60 words)
* Append all valid chunks to final list
