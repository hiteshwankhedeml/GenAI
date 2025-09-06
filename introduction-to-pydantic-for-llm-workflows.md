# Introduction to Pydantic for LLM Workflows

* The simplest way to get structured output is to ask for it in the prompt
* This can be specified in json format
* It can come pretty close to the specification, but not always perfect
* With pydantic, we define data models, that specifies the structure and type of data expected
* Emailstr is a special data type for email
* Literal can only take one of the specified values
* We can pass pydantic model along with the prompt to the LLM
*

    <figure><img src=".gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>



There are 2 ways to validate the output:

* We get the LLM output ⇒ Pass to Pydantic model for validation ⇒ If there is an error, then it is passed to LLM for correction
* We can also pass Prompt + Pydantic model to LLM, so that it knows what output is expected
