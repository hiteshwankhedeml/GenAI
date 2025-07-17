# 🟢 Reciprocal Rank Fusion - Calculation

Hybrid search combines **syntactic search** (BM25, TF-IDF, Sparse Embeddings like SPLADE) with **semantic search** (dense embeddings like SBERT, OpenAI).

**RRF Formula:**

<mark style="color:purple;background-color:purple;">**RRF Score(d)=∑**</mark>$$sum_{i=1}^{n} \frac{1}{k + rank_i(d)}$$

where k=60

k = 60 commonly.

**Example:**

| Document | BM25 Rank | Dense Rank | BM25 Score | Dense Score | Total RRF Score |
| -------- | --------- | ---------- | ---------- | ----------- | --------------- |
| A        | 1         | 3          | 0.01639    | 0.01587     | 0.03226         |
| B        | 2         | 1          | 0.01613    | 0.01639     | 0.03252         |
| C        | 3         | 4          | 0.01587    | 0.01562     | 0.03149         |
| D        | 4         | 2          | 0.01562    | 0.01613     | 0.03175         |

**Final Ranking:** B > A > D > C.\
Hybrid search ensures both keyword precision and semantic understanding.
