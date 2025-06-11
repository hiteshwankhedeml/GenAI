# 🟢 Text Splitting - Recursive Character Text Splitter

* <mark style="color:purple;background-color:purple;">**Every LLM model has limitation of the context size, so we divide the document into smaller chunks**</mark>
* This text splitter is the recommended one for generic text.
* It is parameterized by a list of characters. It tries to split on them in order until the chunks are small enough. The default list is \["\n\n", "\n", " ", ""].
* <mark style="color:purple;background-color:purple;">**This has the effect of trying to keep all paragraphs (and then sentences, and then words) together as long as possible, as those would generically seem to be the strongest semantically related pieces of text.**</mark>
* <mark style="color:purple;background-color:purple;">**How the text is split: by list of characters.**</mark>
* <mark style="color:purple;background-color:purple;">**How the chunk size is measured: by number of characters.**</mark>
* Return type of docs is documents, so we need to use split\_documents here

```python
## Reading a PDf File
from langchain_community.document_loaders import PyPDFLoader
loader=PyPDFLoader('attention.pdf')
docs=loader.load()
docs

from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter=RecursiveCharacterTextSplitter(chunk_size=500,chunk_overlap=50)
final_documents=text_splitter.split_documents(docs)
final_documents # This will also be Of type documents
print(final_documents[0]) # TO check the documents
print(final_documents[1])

## Text Loader
from langchain_community.document_loaders import TextLoader

loader=TextLoader('speech.txt')
docs=loader.load()
docs

## We can also read using open function
speech=""
with open("speech.txt") as f:
    speech=f.read()

text_splitter=RecursiveCharacterTextSplitter(chunk_size=100,chunk_overlap=20)
text=text_splitter.create_documents([speech])
print(text[0])
print(text[1])
```
