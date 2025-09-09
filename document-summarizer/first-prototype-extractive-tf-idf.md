# First Prototype (Extractive - Tf-IDF)

* Used nltk sent\_tokenize, it splits based on punctuations like ! , .
* It knows not to split at Mr. / e.g. / a.m. etc
* Started with extractive summarization
* We calculated tf-idf of sentences
* We find cosine similarity among sentences
* Then using centrality we found importance of each sentence
* We pick top n important sentences
* When we applied TF-IDF to legal contracts, we realized critical clauses like ‘penalty’ or ‘deadline’ often appeared only once, so TF-IDF would undervalue them. To fix this, we introduced a hybrid approach — TF-IDF scoring combined with domain-specific keyword boosting
*

```python
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np
import nltk

# Step 1: Tokenize sentences
sentences = nltk.sent_tokenize(document_text)

# Step 2: Compute TF-IDF
vectorizer = TfidfVectorizer()
X = vectorizer.fit_transform(sentences)

# Step 3: Compute similarity between sentences
similarity_matrix = cosine_similarity(X)

# Step 4: Rank sentences
sentence_scores = similarity_matrix.sum(axis=1)

# Step 5: Pick top N sentences
top_n_sentences = np.argsort(sentence_scores)[-3:]
summary = " ".join([sentences[i] for i in sorted(top_n_sentences)])
```

**Centrality:**

*   The more a sentence is similar to other, the more it means it's talking about the central topic of the document

    | Sentences | S1   | S2   | S3   | S4   | S5   | S6   | S7   | S8   | S9   | S10  |
    | --------- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
    | **S1**    | 1.0  | 0.85 | 0.60 | 0.10 | 0.15 | 0.70 | 0.20 | 0.10 | 0.25 | 0.30 |
    | **S2**    | 0.85 | 1.0  | 0.65 | 0.05 | 0.10 | 0.80 | 0.15 | 0.05 | 0.20 | 0.25 |
    | **S3**    | 0.60 | 0.65 | 1.0  | 0.10 | 0.15 | 0.55 | 0.20 | 0.15 | 0.30 | 0.25 |
    | **S4**    | 0.10 | 0.05 | 0.10 | 1.0  | 0.75 | 0.05 | 0.60 | 0.70 | 0.15 | 0.55 |
    | **S5**    | 0.15 | 0.10 | 0.15 | 0.75 | 1.0  | 0.10 | 0.50 | 0.65 | 0.20 | 0.60 |
    | **S6**    | 0.70 | 0.80 | 0.55 | 0.05 | 0.10 | 1.0  | 0.15 | 0.05 | 0.25 | 0.30 |
    | **S7**    | 0.20 | 0.15 | 0.20 | 0.60 | 0.50 | 0.15 | 1.0  | 0.65 | 0.30 | 0.55 |
    | **S8**    | 0.10 | 0.05 | 0.15 | 0.70 | 0.65 | 0.05 | 0.65 | 1.0  | 0.25 | 0.60 |
    | **S9**    | 0.25 | 0.20 | 0.30 | 0.15 | 0.20 | 0.25 | 0.30 | 0.25 | 1.0  | 0.40 |
    | **S10**   | 0.30 | 0.25 | 0.25 | 0.55 | 0.60 | 0.30 | 0.55 | 0.60 | 0.40 | 1.0  |
* **Example (sum of each row):**
  * S1 → **4.25**
  * S2 → **4.05**
  * S3 → **3.95**
  * S4 → **4.65**
  * S5 → **4.50**
  * S6 → **4.20**
  * S7 → **4.30**
  * S8 → **4.15**
  * S9 → **3.30**
  * S10 → **4.75**



**Domain Boosting:**

* We calculated centrality
* After that for each sentence we also calculated domain boosting score
* As per the no. of important keywords it contains, for each sentence we will find a score
* Then we will do weighted average of centrality and domain boosting
* Final Score (0.7C + 0.3D)



**Handling single words:**

* In resumes sometimes, single words appear on multiple lines
* We checked if there are consecutive single words, then we showed it as single line in summary



**Issues:**

* This approach not worked for resumes, as each sentence carry different information
* If document is scanned, then OCR not used to be correct
* Did not take meaning into account
