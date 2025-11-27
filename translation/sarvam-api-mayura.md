# Sarvam API - Mayura

* Released in 2024
* Accessed via API
* Supported 11 languages
* The maximum input is 1000 characters
* It has script control with options for Roman, native, and spoken forms ⇒ Roman, Fully native, Spoken form
* We used fully native script
* We can also specify gender
* We access it using sarvamai package by using key
* **₹20/10K characters**

```python
from sarvamai import SarvamAI

client = SarvamAI(
    api_subscription_key="YOUR_API_SUBSCRIPTION_KEY",
)

response = client.text.translate(
    input="",
    source_language_code="",
    target_language_code="",
    mode="formal",
    model="mayura:v1",
    output_script="fully-native",
    numerals_format="native",
    speaker_gender="Male",
    enable_preprocessing=False
)

print(response)
```
