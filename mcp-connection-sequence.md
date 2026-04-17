# 🟢 MCP Connection Sequence

* Every MCP connection must start with a specific three-message handshake

1. <mark style="color:$danger;background-color:purple;">**Initialize Request - Client sends this first**</mark>
2. <mark style="color:$danger;background-color:purple;">**Initialize Result - Server responds with capabilities**</mark>
3. <mark style="color:$danger;background-color:purple;">**Initialized Notification - Client confirms (no response expected)**</mark>

* <mark style="color:$danger;background-color:purple;">**Only after this handshake can you send other requests like tool calls or prompt listings.**</mark>

