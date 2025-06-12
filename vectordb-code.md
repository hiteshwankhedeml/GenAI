# ✈️ VectorDB - Code

* &#x20;Their are different document loaders
* If chunking is required then only do it
* Here since we are not doing chunking, so 77 vectors will be stored in vectorDB for each page

```python
from langchain_community.document_loaders import PyPDFLoader
from langchain_community.document_loaders import PyPDFDirectoryLoader

directory_loader=PyPDFDirectoryLoader("C:\\Complete_Content\\GENERATIVEAI\\NEW_E2E_COURSE\\genai_bootcamp\\data")

len(directory_loader.load()) # 107 -- There are 2 pdfs - 77 + 30 == 107 pages

loader=PyPDFLoader(r"C:\\Complete_Content\\GENERATIVEAI\\NEW_E2E_COURSE\\genai_bootcamp\\data\\llama2.pdf")

dcouments=loader.load()

dcouments

len(dcouments) # 77 pages in the document

dcouments[0].metadata
# {'producer': 'pdfTeX-1.40.25',
#  'creator': 'LaTeX with hyperref',
#  'creationdate': '2023-07-20T00:30:36+00:00',
#  'author': '',
#  'keywords': '',
#  'moddate': '2023-07-20T00:30:36+00:00',
#  'ptex.fullbanner': 'This is pdfTeX, Version 3.141592653-2.6-1.40.25 (TeX Live 2023) kpathsea version 6.3.5',
#  'subject': '',
#  'title': '',
#  'trapped': '/False',
#  'source': 'C:\\\\Complete_Content\\\\GENERATIVEAI\\\\NEW_E2E_COURSE\\\\genai_bootcamp\\\\data\\\\llama2.pdf',
#  'total_pages': 77,
#  'page': 0,
#  'page_label': '1'}

dcouments[0].page_content

# To print all the documents
for doc in dcouments:
    print(doc.page_content)
    print("##################################################")


from langchain_openai import OpenAIEmbeddings
import faiss
from langchain_community.docstore.in_memory import InMemoryDocstore
from langchain_community.vectorstores import FAISS

openai_embedding=OpenAIEmbeddings(model='text-embedding-3-large')
dcouments[0].page_content

dim=len(openai_embedding.embed_query(dcouments[0].page_content)) # 3072

faiss_index=faiss.IndexFlatIP(dim)

vector_store = FAISS(
    embedding_function=openai_embedding,
    index=faiss_index,
    docstore=InMemoryDocstore(),
    index_to_docstore_id={},
)

vector_store.add_documents(dcouments)

vector_store.similarity_search(
    query="what is llama2 and what is a difference between llama2 and mistral?",
    k=1
)
# This details are available on 4 pages, but we can also
# restrict using k


```
