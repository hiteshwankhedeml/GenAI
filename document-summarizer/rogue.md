# 🟢 ROGUE

* **Recall-Oriented Understudy for Gisting Evaluation**
* Family of metrics to evaluate summarization by comparing **candidate summary** with **reference summary**
* Focus: **recall** (how much important info is captured)
* Variants: **ROUGE-1, ROUGE-2, ROUGE-L**
* <mark style="color:purple;background-color:purple;">**If we 10 documents, then we will calculate ROGUE for all 10 and then take average**</mark>
* <mark style="color:purple;background-color:purple;">**Both generated summary and reference summary can have multiple sentences (say 10 each).**</mark>
* <mark style="color:purple;background-color:purple;">**ROUGE does not compare sentence-by-sentence.**</mark>
* <mark style="color:purple;background-color:purple;">**Instead, it treats the entire summary as a single sequence of tokens (words).**</mark>

***

#### 🔹 Variants

* <mark style="color:purple;background-color:purple;">**ROUGE-1**</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">→ unigram (single word) overlap</mark>
* <mark style="color:purple;background-color:purple;">**ROUGE-2**</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">→ bigram (two consecutive words) overlap</mark>
* <mark style="color:purple;background-color:purple;">**ROUGE-L**</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">→ longest common subsequence</mark>
* ROUGE-1 shows recall of important words
* ROUGE-2 shows how well phrases are preserved
* ROUGE-L captures fluency through sequence overlap.

***

#### 🔹 Example

**Reference:** `The cat sat on the mat`\
**Candidate:** `The cat lay on the rug`

**ROUGE-1 (Unigrams)**

* Ref unigrams = {the, cat, sat, on, the, mat}
* Cand unigrams = {the, cat, lay, on, the, rug}
* Overlap = {the, cat, on, the} → 4 words
* Recall = 4/6 = **0.67**
* Precision = 4/6 = **0.67**
* F1 = **0.67**

**ROUGE-2 (Bigrams)**

* Ref bigrams = {the cat, cat sat, sat on, on the, the mat}
* Cand bigrams = {the cat, cat lay, lay on, on the, the rug}
* Overlap = {the cat, on the} → 2 bigrams
* Recall = 2/5 = **0.4**
* Precision = 2/5 = **0.4**
* F1 = **0.4**

**ROUGE-L (LCS)**

* Longest Common Subsequence = `the cat on the` (4 words)
* Recall = 4/6 = **0.67**
* Precision = 4/6 = **0.67**
* F1 = **0.67**

***

#### 🔹 Pros

* Simple, fast, widely used benchmark
* Good for **extractive** summarization

***

#### 🔹 Cons

* Only checks **lexical overlap** (ignores synonyms & meaning)
* Penalizes **abstractive** summaries (different wording but same meaning)
* Multiple valid summaries → but ROUGE only rewards overlap with reference

