# Streamable HTTP

* Provide a clever solution to work around HTTP's limitations
* When you're forced to use `stateless_http=True` or `json_response=True`, you're essentially telling the transport to operate within HTTP's constraints rather than working around them
* Some MCP features like sampling, notifications, and logging rely on the server initiating requests to the client.&#x20;
* However, HTTP is designed for clients to make requests to servers, not the other way around.
* StreamableHTTP solves this with a clever workaround using Server-Sent Events (SSE).
*
