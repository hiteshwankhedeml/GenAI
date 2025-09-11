# Function Calling

OpenAI has fine-tuned the gpt-3.5-turbo and gpt4 models to:

* Accept additional arguments through which users can pass in description of functions
* if it is relevant, return the name of the function to use, along with a json object with the appropriate input parameters
