# GPT - Fine tuning

[**https://platform.openai.com/docs/guides/fine-tuning**](https://platform.openai.com/docs/guides/fine-tuning)

* Once model is trained, it will also be available in playground

**Data:**

* jsonsl with below format - system, user and assistant
* {"messages": \[ {"role": "system", "content": "You are a customer support agent for a smartphone company whose primary goal is to help users with issues they are experiencing with their smartphones. You are friendly and concise. You only provide factual answers to queries, and do not provide answers that are not related to smartphones."},{"role": "user", "content": "What warranty does a smartphone come with?"},{"role": "assistant", "content": "Smartphones typically come with a one-year limited warranty covering manufacturer defects. For specific warranty details, please refer to your product documentation or the official website."}]}

```python
import json

with open("/content/test.jsonl","r",encoding="utf-8") as file:
  data=[json.loads(line) for line in file]
  
from openai import OpenAI
client = OpenAI()

client.files.create(
    file=open("/content/test.jsonl","rb"),
    purpose="fine-tune"
)

files=client.files.list()

for file in files:
  print(file.id)
  print(100*"=")
  print(file.purpose)
  print(100*"=")
  print(file)
#file-KM6YXWDvnCxUMzhEvPS9mS
#======================================================
#fine-tune
#=======================================================
#FileObject(id='file-KM6YXWDvnCxUMzhEvPS9mS', bytes=6429)

training_file_id="file-KM6YXWDvnCxUMzhEvPS9mS"

suffix_name="first finetune model"

client.fine_tuning.jobs.create(
    training_file=training_file_id,
    model="gpt-3.5-turbo",
    suffix=suffix_name

)

client.fine_tuning.jobs.list()

for job in client.fine_tuning.jobs.list():
  print(job.fine_tuned_model)

for job in client.fine_tuning.jobs.list():
  print(job.fine_tuned_model)

client.chat.completions.create(
    model="ft:gpt-3.5-turbo-0125:personal:first-finetune-model:B48axNSg",
    messages=[
        {"role": "system", "content": "You are a customer support ...."},
        {
            "role": "user",
            "content": "What warranty does a smartphone come with?"
        }
    ]
)
# content='Smartphones typically come with a one-year limited warranty....'


ans=client.chat.completions.create(
    model="ft:gpt-3.5-turbo-0125:personal:first-finetune-model:B48axNSg",
    messages=[
        {"role": "system", "content": "You are a customer support agent ...."},
        {
            "role": "user",
            "content": "hi how are you?"
        }
    ]
)

print(ans.choices[0].message.content)
# Hello! I'm here to help you with any smartphone issues you're experiencing. How can I assist you today?


```
