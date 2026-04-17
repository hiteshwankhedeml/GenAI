# 🟢 The STDIO transport

* MCP clients and servers communicate by exchanging JSON messages, but how do these messages actually get transmitted? The communication channel used is called a transport
* <mark style="color:$danger;background-color:purple;">**In Stdio, the client launches the MCP server as a subprocess and communicates through standard input and output streams.**</mark>
* <mark style="color:$danger;background-color:purple;">**Client sends messages to the server using the server's**</mark><mark style="color:$danger;background-color:purple;">**&#x20;**</mark><mark style="color:$danger;background-color:purple;">**`stdin`**</mark>
* <mark style="color:$danger;background-color:purple;">**Server responds by writing to**</mark><mark style="color:$danger;background-color:purple;">**&#x20;**</mark><mark style="color:$danger;background-color:purple;">**`stdout`**</mark>
* <mark style="color:$danger;background-color:purple;">**Either the server or client can send a message at any time**</mark>
* <mark style="color:$danger;background-color:purple;">**Only works when client and server run on the same machine**</mark>
