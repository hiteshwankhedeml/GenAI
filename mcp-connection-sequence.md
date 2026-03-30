# MCP Connection Sequence

* Every MCP connection must start with a specific three-message handshake

1. **Initialize Request** - Client sends this first
2. **Initialize Result** - Server responds with capabilities
3. **Initialized Notification** - Client confirms (no response expected)

* Only after this handshake can you send other requests like tool calls or prompt listings.
