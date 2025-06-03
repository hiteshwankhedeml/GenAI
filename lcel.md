# LCEL

* **Chain** is a pipeline that connects components like Prompt → LLM → Output.
* It defines how input flows through the system to produce an output.
* You can build chains in **two styles**:

***

#### 🔷 Chain (Class-Based Style)

* Uses explicit classes like `LLMChain`, `RetrievalQAChain`, etc.
* More verbose, but familiar for OOP-style coding.
*   Example:

    ```python
    pythonCopyEditchain = LLMChain(prompt=prompt, llm=llm)
    result = chain.run({"country": "France"})
    ```

***

#### 🔶 LCEL (LangChain Expression Language)

* <mark style="color:purple;background-color:purple;">**A simpler, modern way to build chains using the pipe (**</mark><mark style="color:purple;background-color:purple;">**`|`**</mark><mark style="color:purple;background-color:purple;">**) operator.**</mark>
* <mark style="color:purple;background-color:purple;">**More concise and readable.**</mark>
*   Example:

    ```python
    pythonCopyEditchain = prompt | llm
    result = chain.invoke({"country": "France"})
    ```

***
