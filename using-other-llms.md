# 🟢 Using other LLMs

* Can also be used with Ollama
*   <mark style="color:purple;background-color:purple;">**We initilaize LLM and passing to agent**</mark>

    ```python
    from langchain_community.llms import HuggingFaceHub

    llm = HuggingFaceHub(
        repo_id="HuggingFaceH4/zephyr-7b-beta",
        huggingfacehub_api_token="<HF_TOKEN_HERE>",
        task="text-generation",
    )

    ### you will pass "llm" to your agent function

    # Mistral API¶
    OPENAI_API_KEY=your-mistral-api-key
    OPENAI_API_BASE=https://api.mistral.ai/v1
    OPENAI_MODEL_NAME="mistral-small"

    # Cohere
    from langchain_community.chat_models import ChatCohere
    # Initialize language model
    os.environ["COHERE_API_KEY"] = "your-cohere-api-key"
    llm = ChatCohere()

    ### you will pass "llm" to your agent function
    ```
