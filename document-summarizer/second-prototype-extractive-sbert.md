# Second Prototype (Extractive - SBERT)

* 2019
* We upgraded our extractive summarizer using SBERT, which was specifically designed to produce sentence embeddings suitable for semantic similarity.
* &#x20;We use all-MiniLM-L6-v2 for embedding
* Computing similarity for very long documents can be memory-intensive
* Dimension 384
* We had calculated ROGUE and had got around R1 \~ 25, R2 \~ 15, RL \~ 23

```python
# 1️⃣ Import Libraries
from sentence_transformers import SentenceTransformer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np
import nltk

# 2️⃣ Input Document (string)
document_text = """Your long document / contract / resume text here."""

# 3️⃣ Sentence Segmentation
sentences = nltk.sent_tokenize(document_text)  # or use SpaCy/custom rules

# 4️⃣ Load SBERT Model (pretrained)
model = SentenceTransformer('all-MiniLM-L6-v2')  # 2019-era SBERT for interview story

# 5️⃣ Encode Sentences
sentence_embeddings = model.encode(sentences)

# 6️⃣ Compute Similarity Matrix (cosine similarity)
similarity_matrix = cosine_similarity(sentence_embeddings)

# 7️⃣ Centrality Calculation (sum of similarities per sentence)
centrality_scores = similarity_matrix.sum(axis=1)

# 8️⃣ Domain Keyword Boost (optional)
domain_keywords = ['deadline', 'penalty', 'termination', 'confidentiality']
domain_boost = []
for sentence in sentences:
    boost = 0.0
    for keyword in domain_keywords:
        if keyword.lower() in sentence.lower():
            boost += 1.0  # you can tune the boost value
    domain_boost.append(boost)
domain_boost = np.array(domain_boost)

# 9️⃣ Weighted Final Score
alpha = 0.7  # weight for centrality
beta = 0.3   # weight for domain boost
final_scores = alpha * centrality_scores + beta * domain_boost

# 🔟 Pick Top-N Sentences (e.g., 3)
top_n = 3
top_sentence_indices = np.argsort(final_scores)[-top_n:]
summary = " ".join([sentences[i] for i in sorted(top_sentence_indices)])

print("Extractive Summary:\n", summary)

```
