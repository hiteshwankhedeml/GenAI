# 🟢 IndicTrans Inferencing

* <mark style="color:purple;background-color:purple;">**Download indic-en, en-indic, indic-indic models from google storage**</mark>
* <mark style="color:purple;background-color:purple;">**initialize the model by selecting the correct directory path**</mark>
* <mark style="color:purple;background-color:purple;">**pass the sentence to be converted**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**IndicTrans is trained with a max sequence length of 200 tokens (subwords). If your sentence is too long (> 200 tokens), the sentence will be truncated to 200 tokens before translation.**</mark>
* <mark style="color:purple;background-color:purple;">**We use smart chunking**</mark>
  * <mark style="color:purple;background-color:purple;">**Split at . or break**</mark>
  * <mark style="color:purple;background-color:purple;">**If still length is more than 200**</mark>
  * <mark style="color:purple;background-color:purple;">**Then split at ⇒ and, but, however... (As per language we had to maintain different keywords)**</mark>
  * <mark style="color:purple;background-color:purple;">**If that also fails then we split at 180 chars**</mark>
* <mark style="color:purple;background-color:purple;">**For a sentence translation, it took 1 to 2 seconds**</mark>
* <mark style="color:purple;background-color:purple;">**Apply rule-based corrections (specific words, phrasing, terminology).**</mark>
* <mark style="color:purple;background-color:purple;">**Example: if the client wants “application” → “prarthana patra”, add it as a replacement rule.**</mark>

```python
# downloading the indic-en model
!wget https://ai4b-public-nlu-nlg.objectstore.e2enetworks.net/indic2en.zip
!unzip indic2en.zip

# downloading the en-indic model
# !wget https://ai4b-public-nlu-nlg.objectstore.e2enetworks.net/en2indic.zip
# !unzip en2indic.zip

# # downloading the indic-indic model
# !wget https://ai4b-public-nlu-nlg.objectstore.e2enetworks.net/m2m.zip
# !unzip m2m.zip


from indicTrans.inference.engine import Model

indic2en_model = Model(expdir='../indic-en')


ta_sents = ['அவனுக்கு நம்மைப் தெரியும் என்று தோன்றுகிறது',
            "நாங்கள் உன்னைத் பின்பற்றுவோம் (அ) தொடர்வோம். ",
            'உங்களுக்கு உங்கள் அருகில் இருக்கும் ஒருவருக்கோ இத்தகைய அறிகுறிகள் தென்பட்டால், வீட்டிலேயே இருப்பது, கொரோனா வைரஸ் தொற்று பிறருக்கு வராமல் தடுக்க உதவும்.']


indic2en_model.batch_translate(ta_sents, 'ta', 'en')



ta_paragraph = """இத்தொற்றுநோய் உலகளாவிய சமூக மற்றும் பொருளாதார சீர்குலைவை ஏற்படுத்தியுள்ளது.இதனால் பெரும் பொருளாதார மந்தநிலைக்குப் பின்னர் உலகளவில் மிகப்பெரிய மந்தநிலை ஏற்பட்டுள்ளது. இது விளையாட்டு,மத, அரசியல் மற்றும் கலாச்சார நிகழ்வுகளை ஒத்திவைக்க அல்லது ரத்து செய்ய வழிவகுத்தது.
அச்சம் காரணமாக முகக்கவசம், கிருமிநாசினி உள்ளிட்ட பொருட்களை அதிக நபர்கள் வாங்கியதால் விநியோகப் பற்றாக்குறை ஏற்பட்டது."""

indic2en_model.translate_paragraph(ta_paragraph, 'ta', 'en')
```
