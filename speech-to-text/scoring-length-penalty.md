# Scoring - Length Penalty

* **Length penalty exists because short answers should not receive high scores**, even if the semantic match with the expected answer is strong.
* **Reason:** A one-line answer shows low depth, low detail, low clarity — so its final score must be reduced.
* **We apply a simple penalty based on number of sentences or word count** of the candidate’s answer.
* **Short answers → big penalty.**\
  **Long answers → no penalty.**
* **Recommended penalty values (simple and effective):**
  * 1 sentence → **0.3 factor**
  * 2 sentences → **0.5 factor**
  * 3–4 sentences → **0.7 factor**
  * 5+ sentences → **1.0 factor**
*   **Formula after semantic similarity:**

    ```
    final_score = semantic_similarity × length_penalty × 10
    ```
* **Example:**
  * Similarity = 0.9
  * Candidate answer = 1 sentence → penalty = 0.3
  * Final score = 0.9 × 0.3 × 10 = **2.7/10**
* **This ensures fairness:**
  * Even if the meaning matches perfectly, a very short answer never gets a high score.
