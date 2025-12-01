# Sentence Embeddings

* Sentence embeddings represent the **meaning** of an entire sentence as a single numeric vector.
* They are produced by **SentenceTransformer models**, which use only the **encoder** part of a transformer.
* The model generates **contextual embeddings for each token** and then applies **mean pooling** to produce one final sentence vector.
* Similarity between sentences is calculated using **cosine similarity**, which compares the direction of two vectors.
* The MiniLM model (`all-MiniLM-L6-v2`) is commonly used because it is small, fast, and performs well for semantic similarity.
* Maximum input size is about **256 tokens**, roughly **150–200 words**.
* The output embedding size for MiniLM is **384 dimensions**, meaning each sentence becomes a 384-number vector.
* Sentence embeddings allow the system to understand **paraphrasing, synonyms, and meaning**, not just keywords.
