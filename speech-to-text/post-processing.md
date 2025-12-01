# Post Processing

1. **Remove filler words:**

* Remove non-meaningful speech like: _um, uh, like, you know, actually, basically, kind of_
* Add noise to text
* Reduce scoring accuracy
* Do NOT add meaning to the answer
* We had a list of filler words, using which we removed the filler words from the text

2. **Frequency based keep or merge:**

* Check if two words are uncommon but the merged words is common ⇒ prefer merged
* Used zipf\_frequency&#x20;

3. If zipf\_frequency is 0 then used spellchecker&#x20;

```python
from spellchecker import SpellChecker
spell = SpellChecker()
corrected = spell.correction(word)
```



