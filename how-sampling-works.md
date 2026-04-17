# 🟢 How Sampling Works

* <mark style="color:$danger;background-color:purple;">**Server completes its work (like fetching Wikipedia articles)**</mark>
* <mark style="color:$danger;background-color:purple;">**Server creates a prompt asking for text generation**</mark>
* <mark style="color:$danger;background-color:purple;">**Server sends a sampling request to the client**</mark>
* <mark style="color:$danger;background-color:purple;">**Client calls Claude with the provided prompt**</mark>
* <mark style="color:$danger;background-color:purple;">**Client returns the generated text to the server**</mark>
* <mark style="color:$danger;background-color:purple;">**Server uses the generated text in its response**</mark>

