# 🟢 Scoring - Advanced Algo

* **Step 1 — Take the candidate answer (1–2 mins).**
* **Step 2 — Split into sentences.**
* **Step 3 — Form chunks using the 15–60 word rule:**
  * If chunk < 15 words → merge with next sentence.
  * If chunk > 60 words → split.
  * Result = meaningful chunks, each representing one clear idea.
* **Step 4 — For the given question, load 2–5 expected answers.**
  * Each expected answer represents a valid way of answering that question.
* **Step 5 — Convert:**
  * each candidate chunk → embedding
  * each expected answer → embedding
* **Step 6 — Compute semantic similarity between:**
  * every candidate chunk
  * every expected answer
* **Step 7 — Take the highest similarity score.**
  * This represents the best part of the candidate’s answer.
  * We never average, because irrelevant sentences would reduce score.
* **Step 8 — Apply length penalty based on number of sentences: ⇒ We changed it into no. of words**
  * 1 sentence → 0.3
  * 2 sentences → 0.5
  * 3–4 sentences → 0.7
  * 5+ sentences → 1.0
*   **Step 9 — Final score:**

    ```
    final_score = best_similarity × length_penalty × 10
    ```
* **Step 10 — Final output:**
  * A number between **0 and 10** for each technical question.
  * Personal questions get **0** (not scored).
