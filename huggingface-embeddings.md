# 🟢 Huggingface Embeddings

* <mark style="color:purple;background-color:purple;">**Huggingface also has lot of open source embeddings**</mark>

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">**from langchain\_huggingface import HuggingFaceEmbeddings**</mark>
* <mark style="color:purple;background-color:purple;">**Remaining will be same as other**</mark>
* Hugging Face sentence-transformers is a Python framework for state-of-the-art sentence, text and image embeddings.
* One of the embedding models is used in the HuggingFaceEmbeddings class.
* We have also added an alias for SentenceTransformerEmbeddings for users who are more familiar with directly using that package.

```python
import os
from dotenv import load_dotenv
load_dotenv()  #load all the environment variables

os.environ['HF_TOKEN']=os.getenv("HF_TOKEN")

from langchain_huggingface import HuggingFaceEmbeddings
embeddings=HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")

text="this is latest documents"
query_result=embeddings.embed_query(text)
query_result

len(query_result) # 384

doc_result = embeddings.embed_documents([text, "This is not a test document."])

```
