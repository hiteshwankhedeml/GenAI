---
hidden: true
---

# ✈️ Scoring - Chunks

* Candidate answers are long, so comparing the full answer to the expected answer gives weak or meaningless similarity.
* We break the long answer into chunks, where each chunk contains 2–3 sentences.
* Each chunk represents one clean idea, similar to how humans process speech.
* We compare every candidate chunk with the expected answer(s).
* Each comparison gives a similarity score, based on meaning.
* We take the highest similarity score among all chunk comparisons, because that chunk captures the best part of the candidate’s response.
* This ensures fairness, as the system rewards the most relevant part of a long answer and ignores irrelevant portions.
