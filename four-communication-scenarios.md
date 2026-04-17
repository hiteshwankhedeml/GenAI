---
hidden: true
---

# ✈️ Four Communication Scenarios

* With any transport, you need to handle four different communication patterns
* **Client → Server request**: Client writes to stdin
* **Server → Client response**: Server writes to stdout
* **Server → Client request**: Server writes to stdout
* **Client → Server response**: Client writes to stdin
* Other transports like HTTP, we'll encounter limitations where the server cannot always initiate requests to the client
