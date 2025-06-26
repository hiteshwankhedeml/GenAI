# 🟢 Chunking - Code

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">Initialize the Chunker - specify chunk size, overlap</mark>
* <mark style="color:purple;background-color:purple;">pass text to be chunked to the chunker</mark>
* <mark style="color:purple;background-color:purple;">We use split\_text - If we want to chunk a text</mark>
* <mark style="color:purple;background-color:purple;">We use split\_documents - To chunk documents</mark>

```python
from langchain_text_splitters import CharacterTextSplitter

text_splitter = CharacterTextSplitter(separator="\n", chunk_size=50, chunk_overlap=0)
chunks = text_splitter.split_text(text=text)

for i, chunk in enumerate(chunks):
    print(f"Chunk {i+1}: {chunk}\n")

#========================================
from langchain.document_loaders import TextLoader

loader=TextLoader("C:\\genai_bootcamp\\data\\state_of_the_union.txt")
docs=loader.load()
len(docs[0].page_content) # 38540
splitter=CharacterTextSplitter(separator="\n", chunk_size=3000,chunk_overlap=250)
split_docs=splitter.split_documents(docs)
split_docs # 14

#========================================
from langchain_text_splitters import RecursiveCharacterTextSplitter
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=3000,
    chunk_overlap=250,
    length_function=len,
    is_separator_regex=False,
)

# Load example document
with open("C:\\Complete_Content\\GENERATIVEAI\\NEW_E2E_COURSE\\genai_bootcamp\\data\\state_of_the_union.txt") as f:
    state_of_the_union = f.read()

text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=3000,
    chunk_overlap=250,
    length_function=len,
    is_separator_regex=False,
)

texts = text_splitter.create_documents([state_of_the_union])
len(texts) # 15

print(texts[0])
print(texts[1])
```
