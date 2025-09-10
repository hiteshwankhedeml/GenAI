# 🟢 Passing multi strings

*   The benefit of using _multiple strings_ :

    ```python
    varname = "line 1 of text"
              "line 2 of text"
    ```

    versus the _triple quote docstring_:

    ```python
    varname = """line 1 of text
                 line 2 of text
              """
    ```

    is that it can avoid adding those whitespaces and newline characters, <mark style="color:purple;background-color:purple;">**making it better formatted to be passed to the LLM.**</mark>
