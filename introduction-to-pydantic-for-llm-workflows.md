# Introduction to Pydantic for LLM Workflows

* The simplest way to get structured output is to ask for it in the prompt
* This can be specified in json format
* It can come pretty close to the specification, but not always perfect
* With pydantic, we define data models, that specifies the structure and type of data expected
* Emailstr is a special data type for email
* Literal can only take one of the specified values
* We can pass pydantic model along with the prompt to the LLM
*
