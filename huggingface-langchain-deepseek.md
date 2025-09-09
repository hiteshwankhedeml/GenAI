---
hidden: true
---

# ✈️ HuggingFace - LangChain - DeepSeek

* This takes lot of memory and it can crash

```python
from langchain_huggingface import HuggingFacePipeline
from transformers import AutoTokenizer, pipeline, AutoModelForCausalLM,BitsAndBytesConfig

model_name="deepseek-ai/DeepSeek-R1"

from transformers import AutoConfig, AutoModelForCausalLM, AutoTokenizer

def load_deepseek_r1(model_name, trust_remote_code=True):
    config = AutoConfig.from_pretrained(model_name, trust_remote_code=trust_remote_code)
    if hasattr(config, "quantization_config"):
        delattr(config, "quantization_config")

    model = AutoModelForCausalLM.from_pretrained(
        model_name,
        config=config,
        trust_remote_code=trust_remote_code
    )

    tokenizer = AutoTokenizer.from_pretrained(model_name, trust_remote_code=trust_remote_code)

    return model, tokenizer

model, tokenizer = load_deepseek_r1("deepseek-ai/DeepSeek-R1")
```
