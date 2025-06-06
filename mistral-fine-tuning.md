# Mistral - Fine Tuning

* &#x20;Documentation of all the libraries can be seen here - [https://huggingface.co/docs](https://huggingface.co/docs)
*

    | **Library**    | **Usage**                                                              |
    | -------------- | ---------------------------------------------------------------------- |
    | `accelerate`   | Simplifies training on multiple devices (CPU, GPU, TPU)                |
    | `peft`         | Parameter-efficient fine-tuning (e.g., LoRA)                           |
    | `bitsandbytes` | Enables 8-bit/4-bit quantization for memory-efficient model loading    |
    | `transformers` | Core library for using and fine-tuning transformer models              |
    | `trl`          | Fine-tuning LLMs with reinforcement learning (e.g., PPO, SFT)          |
    | `py7zr`        | Extracts `.7z` archive files                                           |
    | `auto-gptq`    | Quantizes LLMs using GPTQ for efficient inference                      |
    | `optimum`      | Hardware-accelerated inference and model export (e.g., ONNX, TensorRT) |
* load\_dataset ⇒ To load the dataset
* Dataset ⇒ Convert any dataset into HF format
* AutoModelForCausal ⇒ This will give all the generation models
* We have to make it in following format
* Human: Summarize this following dialogue: <\<text>>   Assistant: <\<summary>>
* By default pad token is not there in mistral, so we assign eos token to it
* If memory issue faced during loading the model for inferencing then use [https://huggingface.co/TheBloke](https://huggingface.co/TheBloke)
* Here all the quantized models are there, we can download from there and fine tune

```python
#!pip install accelerate peft bitsandbytes 
#!pip install git+https://github.com/huggingface/transformers trl 
#!pip install py7zr auto-gptq optimum

from huggingface_hub import notebook_login
notebook_login()

import torch
from datasets import load_dataset, Dataset
from peft import LoraConfig, AutoPeftModelForCausalLM, prepare_model_for_kbit_training, get_peft_model
from transformers import AutoModelForCausalLM, AutoTokenizer, GPTQConfig, TrainingArguments
from trl import SFTTrainer
import os

data = load_dataset("samsum", split="train")

data_df = data.to_pandas()
data_df
#	id	dialogue	summary
#0	13818513	Amanda: I baked cookies. Do you want some?\r\...	Amanda baked cookies and will bring Jerry some...
#....
#14731	13729017	Georgia: are you ready for hotel hunting? We n...	Georgia and Juliette are 


data_df["text"] = data_df[["dialogue", "summary"]].
apply(lambda x: "###Human: Summarize this following dialogue: " + x["dialogue"] + 
"\n###Assistant: " +x["summary"], axis=1)

#id	dialogue	summary	text
#0	13818513	Amanda: I baked cookies. Do you want some?\r\...	Amanda baked cookies and will bring Jerry some...	###Human: Summarize this following dialogue: A...
#....
#14731	13729017	Georgia: are you ready for hotel hunting? We n...	Georgia and Juliette are looking for a hotel i...	###Human: Summarize this following dialogue: G...

tokenizer = AutoTokenizer.from_pretrained("TheBloke/Mistral-7B-Instruct-v0.1-GPTQ")

tokenizer.eos_token      # '</s>'
tokenizer.eos_token_id   # 2

tokenizer.pad_token
tokenizer.pad_token = tokenizer.eos_token

quantization_config_loading = GPTQConfig(bits=4, disable_exllama=True, tokenizer=tokenizer)

model = AutoModelForCausalLM.from_pretrained(
                          "TheBloke/Mistral-7B-Instruct-v0.1-GPTQ",
                          quantization_config=quantization_config_loading,
                          device_map="auto")

#--- Below will give us Quantized model
# Load a 4-bit quantized model
quantization_config = BitsAndBytesConfig(
    load_in_4bit=True,    # Enable 4-bit quantization
    bnb_4bit_compute_dtype=torch.float16,  # Use fp16 for computation
    bnb_4bit_use_double_quant=True,  # Use double quantization for memory efficiency
)

# Load model and tokenizer
model = AutoModelForCausalLM.from_pretrained(
    "mistralai/Mistral-7B-Instruct-v0.1",
    quantization_config=quantization_config,
    device_map="auto"  # Automatically assigns layers to available GPUs
)

model.config.use_cache=False
model.config.pretraining_tp=1
model.gradient_checkpointing_enable()
model = prepare_model_for_kbit_training(model)

#--- Below will give LoRA -- Which will result in QLoRA
peft_config = LoraConfig(
        r=16, lora_alpha=16, lora_dropout=0.05, bias="none", task_type="CAUSAL_LM", target_modules=["q_proj", "v_proj"]
    )
model = get_peft_model(model, peft_config)

training_arguments = TrainingArguments(
        output_dir="mistral-finetuned-samsum",
        per_device_train_batch_size=8,
        gradient_accumulation_steps=1,
        optim="paged_adamw_32bit",
        learning_rate=2e-4,
        lr_scheduler_type="cosine",
        save_strategy="epoch",
        logging_steps=100,
        num_train_epochs=1,
        max_steps=250,
        fp16=True,
        push_to_hub=True,
        report_to="none"
  )
  
trainer.train()

! cp -r /content/mistral-finetuned-samsum /content/drive/MyDrive/

trainer.push_to_hub()
```

* **Inference:**

```python
from peft import AutoPeftModelForCausalLM
from transformers import GenerationConfig
from transformers import AutoTokenizer
import torch
tokenizer = AutoTokenizer.from_pretrained("/content/mistral-finetuned-samsum")

inputs = tokenizer("""
###Human: Summarize this following dialogue: Sunny: I'm at the railway station in Chennai Karthik: No problems so far? Sunny: no, everything's going smoothly Karthik: good. lets meet there soon!
###Assistant: """, return_tensors="pt").to("cuda")

model = AutoPeftModelForCausalLM.from_pretrained(
    "/content/mistral-finetuned-samsum",
    low_cpu_mem_usage=True,
    return_dict=True,
    torch_dtype=torch.float16,
    device_map="cuda")

generation_config = GenerationConfig(
    do_sample=True,
    top_k=1,
    temperature=0.1,
    max_new_tokens=25,
    pad_token_id=tokenizer.eos_token_id
)

import time
st_time = time.time()
outputs = model.generate(**inputs, generation_config=generation_config)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
print(time.time()-st_time)
## Human: Summarize this following dialogue: Vasanth: I'm at the railway station in Chennai Karthik: No problems so far? Vasanth: no, everything's going smoothly Karthik: good. lets meet there soon!
## Assistant:  Vasanth is at the railway station in Chennai. Everything is going smoothly. He will meet Karthik soon.
## 29.11742496490478
```
