# 🟢 Roots

* <mark style="color:$danger;">**Roots are a way to grant MCP servers access to specific files and folders on your local machine.**</mark>

1. User asks to convert a video file
2. Claude calls `list_roots` to see what directories it can access
3. Claude calls `read_dir` on accessible directories to find the file
4. Once found, Claude calls the conversion tool with the full path

*

    <figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
