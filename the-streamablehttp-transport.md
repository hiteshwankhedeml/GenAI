# 🟢 The StreamableHTTP transport

* <mark style="color:$danger;background-color:purple;">**Enables MCP clients to connect to remotely hosted servers over HTTP connections**</mark>
* some configuration settings can significantly limit your MCP server's functionality.&#x20;
* If your application works perfectly with standard I/O transport locally but breaks when deployed with HTTP transport, this is likely the culprit
* Two key settings control how the streamable HTTP transport behaves:
  * `stateless_http` - Controls connection state management
  * `json_response` - Controls response format handling
* By default, both settings are `false`, but certain deployment scenarios may force you to set them to `true`. When enabled, these settings can break core functionality like progress notifications, logging, and server-initiated requests.
* <mark style="color:$danger;background-color:purple;">**Servers cannot easily initiate requests to clients (clients don't have known URLs)**</mark>
* <mark style="color:$danger;background-color:purple;">**Response patterns from client back to server become problematic**</mark>
* <mark style="color:$danger;background-color:purple;">**The following message types become difficult to implement with plain HTTP:**</mark>
  * <mark style="color:$danger;background-color:purple;">**Server-initiated requests: Create Message requests, List Roots requests**</mark>
  * <mark style="color:$danger;background-color:purple;">**Notifications: Progress notifications, Logging notifications, Initialized notifications, Cancelled notifications**</mark>
*
