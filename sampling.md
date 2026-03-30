# Sampling

* Allows a server to access a language model like Claude through a connected MCP client.&#x20;
* Instead of the server directly calling Claude, it asks the client to make the call on its behalf.&#x20;
* This shifts the responsibility and cost of text generation from the server to the client.
* Sampling is most valuable when building publicly accessible MCP servers.&#x20;
* You don't want random users generating unlimited text at your expense.&#x20;
* By using sampling, each client pays for their own AI usage while still benefiting from your server's functionality
*

    <figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
