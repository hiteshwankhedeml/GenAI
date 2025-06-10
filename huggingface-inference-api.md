# 🟢 HuggingFace - Inference API

* [https://huggingface.co/docs/api-inference/en/index](https://huggingface.co/docs/api-inference/en/index)
* <mark style="color:purple;background-color:purple;">**Can be done using requests or huggingface-inference client**</mark>

```python
import requests

API_URL = "https://api-inference.huggingface.co/models/cardiffnlp/twitter-roberta-base-sentiment-latest"
headers = {"Authorization": "Bearer hf_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"}
payload = {
    "inputs": "Today is a great day",
}

response = requests.post(API_URL, headers=headers, json=payload)
response.json()
```

```python
from huggingface_hub import InferenceClient

client = InferenceClient(
    "cardiffnlp/twitter-roberta-base-sentiment-latest",
    token="hf_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
)

client.text_classification("Today is a great day")
```
