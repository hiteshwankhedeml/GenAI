# Scoring - Advanced - Code

```python
# -----------------------------------------------------------
# PHASE-3 SEMANTIC SCORING ENGINE (CHUNKING + PENALTY)
# -----------------------------------------------------------

from sentence_transformers import SentenceTransformer, util
import re

# Load embedding model
model = SentenceTransformer("sentence-transformers/all-MiniLM-L6-v2")

# Expected answers per question category
EXPECTED = {
    "current job role": [
        "I manage sales targets and handle my territory.",
        "I work with distributors and ensure coverage.",
        "I handle customer issues and coordinate with my team."
    ],
    "difficult customer": [
        "I resolved the customer's issue professionally.",
        "I calmed the customer and handled the situation carefully.",
        "I understood the problem and provided the right solution."
    ]
}

# -------------------------------
# Helper: Split into sentences
# -------------------------------
def split_sentences(text):
    sents = re.split(r'[.!?]+', text)
    sents = [s.strip() for s in sents if s.strip()]
    return sents

# -------------------------------
# Helper: Chunk formation (15–60 words)
# -------------------------------
def create_chunks(text, MIN_WORDS=15, MAX_WORDS=60):
    sentences = split_sentences(text)
    if len(sentences) <= 2:
        return [text]

    chunks = []
    current = []
    current_len = 0

    for s in sentences:
        words = s.split()
        wlen = len(words)

        # If sentence itself is very long ( > MAX_WORDS )
        if wlen > MAX_WORDS:
            # push current chunk if exists
            if current:
                chunks.append(" ".join(current))
                current = []
                current_len = 0

            # split long sentence
            for i in range(0, wlen, MAX_WORDS):
                chunk_words = words[i:i+MAX_WORDS]
                chunks.append(" ".join(chunk_words))
            continue

        # Normal sentence case
        if current_len + wlen <= MAX_WORDS:
            current.append(s)
            current_len += wlen
        else:
            # push current chunk if valid
            if current_len >= MIN_WORDS:
                chunks.append(" ".join(current))
                current = [s]
                current_len = wlen
            else:
                # too small → add even if it crosses MAX_WORDS
                current.append(s)
                current_len += wlen

    # add leftover chunk
    if current:
        if chunks and len(current_len) < MIN_WORDS:
            last = chunks.pop()
            chunks.append(last + " " + " ".join(current))
        else:
            chunks.append(" ".join(current))

    return chunks

# -------------------------------
# Helper: Length penalty
# -------------------------------
def length_penalty(text):
    sents = split_sentences(text)
    n = len(sents)

    if n <= 1:
        return 0.3
    if n == 2:
        return 0.5
    if 3 <= n <= 4:
        return 0.7
    return 1.0

# -------------------------------
# Main Scoring Function
# -------------------------------
def semantic_score(question, answer):
    q = question.lower()

    # find category
    cat = None
    for key in EXPECTED:
        if key in q:
            cat = key
            break
    if cat is None:
        return 0.0

    # chunk candidate answer
    chunks = create_chunks(answer)

    best_sim = 0

    # embed expected answers once
    exp_embs = [model.encode(e, convert_to_tensor=True) for e in EXPECTED[cat]]

    # compare each chunk against each expected answer
    for ch in chunks:
        ch_emb = model.encode(ch, convert_to_tensor=True)
        for exp_emb in exp_embs:
            sim = util.cos_sim(ch_emb, exp_emb).item()
            best_sim = max(best_sim, sim)

    # apply length penalty
    penalty = length_penalty(answer)

    # final 0–10 score
    final_score = best_sim * penalty * 10
    return round(final_score, 1)

# -------------------------------
# Example Usage
# -------------------------------
if __name__ == "__main__":
    q = "Explain your current job role"
    a = """
    I handle customer issues regularly. I also work with the distributors 
    to ensure daily targets are met. I coordinate with my team and report 
    the daily progress to my manager.
    """

    print("Final Score:", semantic_score(q, a))

```
