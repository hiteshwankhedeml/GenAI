# 🟢 MCP Clients

* <mark style="color:$danger;background-color:purple;">**MCP client serves as the communication bridge between your server and MCP servers**</mark>
* <mark style="color:$danger;background-color:purple;">**Client and server can communicate over different protocols depending on your setup**</mark>
* The most common setup runs both the MCP client and server on the same machine, communicating through standard input/output. But you can also connect them over:
  * HTTP
  * WebSockets
  * Various other network protocols
* Once connected, the client and server exchange specific message types defined in the MCP specification
