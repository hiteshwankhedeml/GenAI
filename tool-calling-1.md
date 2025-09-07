# Tool Calling

* &#x20;We will be taking user input and validate against UserInput model
* We will pass this to LLM and ask it to return valid instance of CustomerQuery Model
* This will be passed to another LLM with tool definitions
  *

      <figure><img src=".gitbook/assets/{2FE97E82-67B8-4083-8557-7B87BE49DF67}.png" alt=""><figcaption></figcaption></figure>
* Job of the LLM is to return parameters for a tool if it is to be called
* We will take the response and first use the pydantic data model for tools and validate the parameters are in order to call the functions
* Call the tools and get output
* This will be passed to another LLM

<figure><img src=".gitbook/assets/{005B1057-5E10-4899-ADAF-F7B138AC01D5}.png" alt=""><figcaption></figcaption></figure>
