# 🟢 Creating a Chatbot

* <mark style="color:purple;background-color:purple;">**Notice how chathistory is getting saved here in list and also how entire history is being passed to LLM in invoke**</mark>

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**list of messages - start with a system prompt**</mark>
* <mark style="color:purple;background-color:purple;">**append human messages to list**</mark>
* <mark style="color:purple;background-color:purple;">**pass the list of messages to the llm**</mark>
* <mark style="color:purple;background-color:purple;">**append ai message to list**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**Repeat**</mark>

```python
chat_history = [
    SystemMessage(content="you are a helpful assistant")
]

while True:
    user_input=input("user_input: ")
    chat_history.append(HumanMessage(content=user_input))
    if user_input=="exit":
        break
    result=openai_model.invoke(chat_history)
    chat_history.append(AIMessage(result.content))
    print("AI Generated Answer:", result.content)

# AI Generated Answer: The capital of India is New Delhi.
# AI Generated Answer: The Prime Minister of India is Narendra Modi, as of my knowledge update in October 2021.
# AI Generated Answer: Narendra Modi was born on September 17, 1950. So, as of October 2021, he is 71 years old.
# AI Generated Answer: Narendra Modi was born on September 17, 1950. As of 2022, he is 72 years old. Please note that the information is based on the current year, so you may need to adjust his age based on the current year.
# AI Generated Answer: If Narendra Modi was born in 1950, then in 2030, he will be 80 years old.

print(chat_history)
# [SystemMessage(content='you are a helpful assistant', additional_kwargs={}, response_metadata={}),
# HumanMessage(content='what is a capital of india?', additional_kwargs={}, response_metadata={}),
# AIMessage(content='The capital of India is New Delhi.', additional_kwargs={}, response_metadata={}),
# HumanMessage(content='who is a prime minister of india?', additional_kwargs={}, response_metadata={}),
# AIMessage(content='The Prime Minister of India is Narendra Modi, as of my knowledge update in October 2021.\n', additional_kwargs={}, response_metadata={}),
# HumanMessage(content='what is a age of him?', additional_kwargs={}, response_metadata={}),
# AIMessage(content='Narendra Modi was born on September 17, 1950. So, as of October 2021, he is 71 years old.', additional_kwargs={}, response_metadata={}),
# HumanMessage(content='what is a age of him?', additional_kwargs={}, response_metadata={}),
# AIMessage(content='Narendra Modi was born on September 17, 1950. As of 2022, he is 72 years old. Please note that the information is based on the current year, so you may need to adjust his age based on the current year.', additional_kwargs={}, response_metadata={}),
# HumanMessage(content='so what will a age in 2030?', additional_kwargs={}, response_metadata={}),
# AIMessage(content='If Narendra Modi was born in 1950, then in 2030, he will be 80 years old.', additional_kwargs={}, response_metadata={}), HumanMessage(content='exit', additional_kwargs={}, response_metadata={})]
```
