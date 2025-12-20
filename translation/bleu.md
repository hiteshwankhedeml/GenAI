# 🟢 BLEU

**Typical real-world BLEU ranges**

* **10–20** → weak translation
* <mark style="color:purple;background-color:purple;">**20–30 → acceptable**</mark>
* <mark style="color:purple;background-color:purple;">**30–40 → strong**</mark>
* **40+** → very rare, usually constrained domains



<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**Break candidate sentence into n-grams (words, word pairs, etc.)**</mark>
* <mark style="color:purple;background-color:purple;">**Break reference sentence into n-grams**</mark>
* <mark style="color:purple;background-color:purple;">**Count how many n-grams match**</mark>
* <mark style="color:purple;background-color:purple;">**Limit (clip) matches to how many times they appear in reference**</mark>\ <mark style="color:purple;background-color:purple;">**→ this is modified n-gram precision**</mark>
* <mark style="color:purple;background-color:purple;">**Divide clipped matches by total candidate n-grams**</mark>
* <mark style="color:purple;background-color:purple;">**Do this for unigram, bigram, …**</mark>
* <mark style="color:purple;background-color:purple;">**Take geometric mean of these scores**</mark>
* <mark style="color:purple;background-color:purple;">**Apply brevity penalty if candidate is shorter**</mark>
* <mark style="color:purple;background-color:purple;">**Multiply both to get BLEU score**</mark>



