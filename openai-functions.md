# OpenAI Functions

* Here we have hardcoded function current\_weather ⇒ in actual we will be calling API inside this
* To define a function we need
  * name
  * description
  * parameters
  * Required parameters
* Description are very important as it will be passed to LLM
* Initially there will be nothing in response, as it will not generate as it does not generates and just gives the name of function which is to be called
* if the message passed is not related to weather, then in response there will be content and there won't be any function calling
* By default function\_call is auto, this means that language model chooses
* If we pass function\_call as none, this means that none of the function will be used
* If we pass name of the function\_call then it will force the function to be called
* Function calls takes up tokens
* We pass function call in the message and the to the llm
* llm will take the output of function and will convert it into nice message

```python




```
