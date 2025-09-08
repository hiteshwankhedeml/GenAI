# 🔴 Using Ollama Remotely

| **Action**                     | **Command / Code / Note**                                                                                                            |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| Install & Run Ollama on Remote | `ollama serve`                                                                                                                       |
| Default API Endpoint           | `http://<REMOTE_IP>:11434`                                                                                                           |
| Expose API - Option 1          | <mark style="color:purple;background-color:purple;">**Open port 11434 on cloud VM**</mark>                                           |
| Expose API - Option 2          | `ngrok http 11434` → gives **public HTTPS URL**                                                                                      |
| LangChain Integration          |                                                                                                                                      |
| Import Ollama                  | `from langchain_community.llms import Ollama`                                                                                        |
| Create Client                  | <mark style="color:purple;background-color:purple;">**`llm = Ollama(model='codeguru', base_url='http://<REMOTE_IP>:11434')`**</mark> |
| Invoke                         | `llm.invoke("your query")`                                                                                                           |
| Optional Security              | Use **Nginx + Basic Auth**, **VPN**, or **SSH tunnel**                                                                               |
