# ✈️ ChromaDB - Code

```python
from langchain_community.document_loaders import PyPDFDirectoryLoader
from langchain.vectorstores import Chroma

directory_loader=PyPDFDirectoryLoader("C:\\genai_bootcamp\\data")

len(directory_loader.load()) # 107

dcouments=directory_loader.load()

from langchain_text_splitters import RecursiveCharacterTextSplitter
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=2000,
    chunk_overlap=300,
    length_function=len,
    is_separator_regex=False,
)

further_split_doc=text_splitter.split_documents(dcouments)

len(further_split_doc) # 231

persist_directory="vdb_latest"

chroma_vdb=Chroma.from_documents(documents=further_split_doc,
                      embedding=openai_embedding,
                      persist_directory=persist_directory,
                      )

chroma_vdb.persist()

#--------------------Loading the DB
vdb=Chroma(persist_directory=persist_directory,
        embedding_function=openai_embedding)
```
