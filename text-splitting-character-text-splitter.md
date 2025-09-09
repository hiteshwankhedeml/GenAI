---
hidden: true
---

# ⚔️ Text Splitting - Character text splitter

* This is the simplest method. This splits based on a given character sequence, which defaults to "\n\n". Chunk length is measured by number of characters.

1. <mark style="color:purple;background-color:purple;">**How the text is split: by single character separator.**</mark>
2. <mark style="color:purple;background-color:purple;">**How the chunk size is measured: by number of characters.**</mark>

```python
from langchain_community.document_loaders import TextLoader

loader=TextLoader('speech.txt')
docs=loader.load()
docs

from langchain_text_splitters import CharacterTextSplitter
text_splitter=CharacterTextSplitter(separator="\n\n",chunk_size=100,chunk_overlap=20)
text_splitter.split_documents(docs)

speech=""
with open("speech.txt") as f:
    speech=f.read()

text_splitter=CharacterTextSplitter(chunk_size=100,chunk_overlap=20)
text=text_splitter.create_documents([speech])
print(text[0])
print(text[1])
```
