# The STDIO transport

* MCP clients and servers communicate by exchanging JSON messages, but how do these messages actually get transmitted? The communication channel used is called a transport
* In Stdio, the client launches the MCP server as a subprocess and communicates through standard input and output streams.
* Client sends messages to the server using the server's `stdin`
* Server responds by writing to `stdout`
* Either the server or client can send a message at any time
* Only works when client and server run on the same machine
*
