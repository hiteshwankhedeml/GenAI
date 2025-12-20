# 🟢 Sarvam API - Mayura

* <mark style="color:purple;background-color:purple;">**Released in 2024**</mark>
* <mark style="color:purple;background-color:purple;">**Accessed via API**</mark>
* <mark style="color:purple;background-color:purple;">**Supported 11 languages**</mark>
* <mark style="color:purple;background-color:purple;">**The maximum input is 1000 characters**</mark>
* It has script control with options for Roman, native, and spoken forms ⇒ Roman, Fully native, Spoken form
* We used fully native script
* <mark style="color:purple;background-color:purple;">**We can also specify gender**</mark>
* <mark style="color:purple;background-color:purple;">**We access it using sarvamai package by using key**</mark>
* <mark style="color:purple;background-color:purple;">**₹20/10K characters**</mark>

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
