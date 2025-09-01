# 🟢 Runnable Pass Through

* <mark style="color:purple;background-color:purple;">**`RunnablePassthrough`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**simply returns the input as the output without modification.**</mark>
* <mark style="color:purple;background-color:purple;">**Useful when you want to forward data through a chain without changing it.**</mark>
*   Example:

    ```python
    from langchain_core.runnables import RunnablePassthrough

    chain = RunnablePassthrough()
    print(chain.invoke("hello"))
    # Output: "hello"
    ```
* <mark style="color:purple;background-color:purple;">**`.assign`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**is used with runnables to keep the original input and add new keys/values.**</mark>
*   Example with `.assign`:

    ```python
    from langchain_core.runnables import RunnablePassthrough

    chain = RunnablePassthrough().assign(
        added=lambda x: x.upper()
    )
    print(chain.invoke("hello"))
    # Output: {"input": "hello", "added": "HELLO"}
    ```
* Together, `RunnablePassthrough` ensures the original input is retained, and `.assign` appends new computed fields.
