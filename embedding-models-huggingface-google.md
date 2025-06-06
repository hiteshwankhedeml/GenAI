# Embedding Models - HuggingFace/Google

```python
from langchain_huggingface import HuggingFaceEmbeddings
huggingface_embeddings=HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")

from langchain_google_genai import GoogleGenerativeAIEmbeddings
google_embeddings = GoogleGenerativeAIEmbeddings(model="models/embedding-001")
```
