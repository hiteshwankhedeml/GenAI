---
hidden: true
---

# ✈️ AWS Bedrock

* Bedrock is the easiest way to <mark style="color:purple;background-color:purple;">**build and scale Gen AI applications in AWS**</mark>
* <mark style="color:purple;background-color:purple;">**It provides platforms where all the models will be available, and using their APIs all can be used**</mark>
* OpenAI is not available as of now
* Different foundation models available&#x20;
* This is paid service
* <mark style="color:purple;background-color:purple;">**In AWS ⇒ Create IAM user ⇒ Attach policies ⇒ Create awscli access key**</mark>
* <mark style="color:purple;background-color:purple;">**Command prompt ⇒ aws configure ⇒ Enter key here**</mark>
* If model access is not given, then in AWS request access

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">use boto3 package</mark>
* <mark style="color:purple;background-color:purple;">Create client using bedrock=boto3.client(service\_name="bedrock-runtime")</mark>
* <mark style="color:purple;background-color:purple;">Invoke LLM by passing payload, model, payload to bedrock</mark>

```python
import boto3
import json

prompt_data="""
Act as a Shakespeare and write a poem on Genertaive AI
"""

bedrock=boto3.client(service_name="bedrock-runtime")

payload={
    "prompt":"[INST]"+ prompt_data +"[/INST]",
    "max_gen_len":512,
    "temperature":0.5,
    "top_p":0.9
}
body=json.dumps(payload)
model_id="meta.llama2-70b-chat-v1"
response=bedrock.invoke_model(
    body=body,
    modelId=model_id,
    accept="application/json",
    contentType="application/json"
)

response_body=json.loads(response.get("body").read())
repsonse_text=response_body['generation']
print(repsonse_text)
```

* Stabble diffusion

```python
import boto3
import json
import base64
import os

prompt_data = """
provide me an 4k hd image of a beach, also use a blue sky rainy season and
cinematic display
"""
prompt_template=[{"text":prompt_data,"weight":1}]
bedrock = boto3.client(service_name="bedrock-runtime")
payload = {
    "text_prompts":prompt_template,
    "cfg_scale": 10,
    "seed": 0,
    "steps":50,
    "width":512,
    "height":512

}

body = json.dumps(payload)
model_id = "stability.stable-diffusion-xl-v0"
response = bedrock.invoke_model(
    body=body,
    modelId=model_id,
    accept="application/json",
    contentType="application/json",
)

response_body = json.loads(response.get("body").read())
print(response_body)
artifact = response_body.get("artifacts")[0]
image_encoded = artifact.get("base64").encode("utf-8")
image_bytes = base64.b64decode(image_encoded)

# Save image to a file in the output directory.
output_dir = "output"
os.makedirs(output_dir, exist_ok=True)
file_name = f"{output_dir}/generated-img.png"
with open(file_name, "wb") as f:
    f.write(image_bytes)
```
