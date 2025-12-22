# 🟢 LLM - On Prem

* A VM or physical server that has a GPU attached (like NVIDIA A10, A100, T4, L4, etc.) is used.
* Inside that VM, you install the LLM runtime (vLLM / transformers / llama.cpp) and download the model weights.
* Install vLLM server on your GPU VM and start it as an OpenAI-compatible endpoint (vLLM exposes `/v1/chat/completions`).
* In LangChain, use the ChatOpenAI class but point the `base_url` to your local vLLM server instead of OpenAI.
* Set `api_key="dummy"` because vLLM doesn’t need authentication but LangChain requires the field.



