# GPT - Prompt

* &#x20;System to define overall behaviour of the model
* User prompt means the question which is to be asked
* Assistant means LLM generated answer

```python
!pip install openai

from google.colab import userdata
OPENAI_API_KEY=userdata.get('OPENAI_API_KEY')

from openai import OpenAI
client = OpenAI()

completion = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {
            "role": "user",
            "content": "what is a championes trophy in cricket how it is different from worldcup."
        }
    ]
)

print(completion.choices[0].message.content)
```
