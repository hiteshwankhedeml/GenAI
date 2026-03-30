# How Sampling Works

* Server completes its work (like fetching Wikipedia articles)
* Server creates a prompt asking for text generation
* Server sends a sampling request to the client
* Client calls Claude with the provided prompt
* Client returns the generated text to the server
* Server uses the generated text in its response

