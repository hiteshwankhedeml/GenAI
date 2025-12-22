# 🟢 ZipF

* <mark style="color:purple;background-color:purple;">**Zipf score tells how common a word is in everyday language**</mark>
* <mark style="color:purple;background-color:purple;">**It is based on how many times the word appears in very large text data**</mark>
* <mark style="color:purple;background-color:purple;">**First, count:**</mark>
  * <mark style="color:purple;background-color:purple;">**How many times the word appears in 1 billion words**</mark>
* <mark style="color:purple;background-color:purple;">**Then, compress that big number:**</mark>
  * <mark style="color:purple;background-color:purple;">**Take the log (base 10) of that count**</mark>
  * This keeps numbers small and easy to compare
* Simple intuition:
  * Very common words → **high Zipf score**
  * Unusual or wrong words → **low Zipf score**
* Example:
  * A word seen **10,000 times** → Zipf score **4**
  * A word seen **100 times** → Zipf score **2**
  * A word seen **1 time** → Zipf score **0**
