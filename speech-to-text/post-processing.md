# 🟢 Post Processing

1. <mark style="color:purple;background-color:purple;">**Remove filler words:**</mark>

* <mark style="color:purple;background-color:purple;">**Remove non-meaningful speech like:**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark>_<mark style="color:purple;background-color:purple;">**um, uh, like, you know, actually, basically, kind of**</mark>_
* <mark style="color:purple;background-color:purple;">**Add noise to text**</mark>
* <mark style="color:purple;background-color:purple;">**Reduce scoring accuracy**</mark>
* <mark style="color:purple;background-color:purple;">**Do NOT add meaning to the answer**</mark>
* <mark style="color:purple;background-color:purple;">**We had a list of filler words, using which we removed the filler words from the text**</mark>

2. <mark style="color:purple;background-color:purple;">**Frequency based keep or merge:**</mark>

* <mark style="color:purple;background-color:purple;">**Check if two words are uncommon but the merged words is common ⇒ prefer merged**</mark>
* <mark style="color:purple;background-color:purple;">**Used zipf\_frequency**</mark>&#x20;

3. <mark style="color:purple;background-color:purple;">**If zipf\_frequency is 0 then used spellchecker**</mark>&#x20;

```python
from spellchecker import SpellChecker
spell = SpellChecker()
corrected = spell.correction(word)
```



