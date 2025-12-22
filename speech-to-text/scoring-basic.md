# 🟢 Scoring - Basic

* <mark style="color:purple;background-color:purple;">**We scored only technical and role-based questions; personal questions were not scored.**</mark>
* <mark style="color:purple;background-color:purple;">**Each technical question had a predefined keyword list created from job descriptions and SME inputs.**</mark>
* <mark style="color:purple;background-color:purple;">**Whisper transcript text was cleaned and normalized before scoring.**</mark>
* <mark style="color:purple;background-color:purple;">**For each question, we counted how many expected keywords appeared in the candidate’s answer.**</mark>
* <mark style="color:purple;background-color:purple;">**Negation handling was applied using a ±3-word window, so keywords in negated sentences (e.g., “I don’t know C++”) were counted as negative, not positive.**</mark>
* <mark style="color:purple;background-color:purple;">**Final question score = positive keyword count − negative keyword count, with a lower limit of zero.**</mark>
* <mark style="color:purple;background-color:purple;">**Each question had its own keyword set, so scoring remained question-specific and explainable.**</mark>
* <mark style="color:purple;background-color:purple;">**Final interview score was the weighted sum of all technical question scores, keeping the system transparent and easy to interpret.**</mark>
* <mark style="color:purple;background-color:purple;">**We calculated overall score on a scale of 10**</mark>
* <mark style="color:purple;background-color:purple;">**We also generated a pdf which had obe**</mark>

```python
# ------------------------------------------
# Phase-2 Scoring Script (Single File)
# ------------------------------------------

# Personal questions – no scoring applied
PERSONAL_QUESTIONS = [
    "tell us about your family",
    "why do you wish to join jio",
    "tell us something about yourself",
    "describe your town",
    "why do you want to be associated with jio"
]

# Technical / role-based keyword banks
KEYWORDS = {
    "current job role": ["responsibility", "role", "sales", "targets", "team", "territory"],
    "achievements": ["achievement", "award", "milestone", "recognition", "target"],
    "sales pitch": ["benefits", "features", "value", "roi", "customer", "retailer"],
    "difficult customer": ["problem", "resolve", "handled", "issue", "situation"],
    "lead generation": ["lead", "prospect", "client", "conversion"],
    "education": ["project", "engineering", "academic", "final", "design"],
    "distributor": ["distributor", "criteria", "market", "territory", "performance"]
}

# Negation words
NEGATIONS = ["no", "not", "dont", "don't", "never", "cannot", "can't"]


# ------------------------------------------------------
# Negation detection using ±3 word window
# ------------------------------------------------------
def is_negated(words, index, window=3):
    start = max(0, index - window)
    end = min(len(words), index + window + 1)

    for i in range(start, end):
        if words[i] in NEGATIONS:
            return True
    return False


# ------------------------------------------------------
# Main scoring function
# ------------------------------------------------------
def score_answer(question, answer):
    q = question.lower().strip()
    ans = answer.lower()
    words = ans.split()

    # 1. No scoring for personal questions
    if any(q.startswith(p) for p in PERSONAL_QUESTIONS):
        return 0

    # 2. Identify keyword category
    selected_keywords = None
    for category in KEYWORDS:
        if category in q:
            selected_keywords = KEYWORDS[category]
            break

    if selected_keywords is None:
        return 0  # No matching keyword category

    positive = 0
    negative = 0

    # 3. Keyword matching + negation handling
    for i, w in enumerate(words):
        for kw in selected_keywords:
            if w == kw:
                if is_negated(words, i):
                    negative += 1
                else:
                    positive += 1

    # 4. Final score = positive - negative (no negatives)
    return max(positive - negative, 0)


# ------------------------------------------------------
# Example usage
# ------------------------------------------------------
if __name__ == "__main__":
    q1 = "Explain your current job role"
    a1 = "I handle sales and territory but I do not manage targets"
    print("Score 1:", score_answer(q1, a1))

    q2 = "Tell 3 things you considered while appointing your last distributor"
    a2 = "We evaluated market performance and territory but not the criteria fully"
    print("Score 2:", score_answer(q2, a2))

```

