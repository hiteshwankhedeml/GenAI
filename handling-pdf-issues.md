# 🟢 Handling PDF Issues

* Store text in complex ways (not just simple text)
* Can have formatting issues
* <mark style="color:purple;background-color:purple;">**May contain scanned images (requiring OCR)**</mark>
* Often have extraction artifacts

**Steps:**

* Load the pdf
* <mark style="color:purple;background-color:purple;">**Clean the text ⇒ if there are any known characters which are read wrong by OCR then replace it**</mark>
* <mark style="color:purple;background-color:purple;">**If the page has less than 50 characters then remove it**</mark>
* Chunk the page

```python
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

from langchain_core.documents import Document
from typing import List
class SmartPDFProcessor:
    """Advanced PDF processing with error handling"""
    def __init__(self,chunk_size=1000,chunk_overlap=100):
        self.chunk_size=chunk_size,
        self.chunk_overlap=chunk_overlap,
        self.text_splitter=RecursiveCharacterTextSplitter(
            chunk_size=chunk_size,
            chunk_overlap=chunk_overlap,
            separators=[" "],

        )

    def process_pdf(self,pdf_path:str)->List[Document]:
        """Process PDF with smart chunking and metadata enhancement"""

        # Laod PDF

        loader=PyPDFLoader(pdf_path)
        pages=loader.load()

        ## Process each page

        processed_chunks=[]

        for page_num,page in enumerate(pages):
            ## clean text
            cleaned_text=self._clean_text(page.page_content)

            # Skip nearly empty pages
            if len(cleaned_text.strip()) < 50:
                continue

            # Create chunks with enhanced metadata
            chunks = self.text_splitter.create_documents(
                texts=[cleaned_text],
                metadatas=[{
                    **page.metadata,
                    "page": page_num + 1,
                    "total_pages": len(pages),
                    "chunk_method": "smart_pdf_processor",
                    "char_count": len(cleaned_text)
                }]
            )
            
            processed_chunks.extend(chunks)

        return processed_chunks

    def _clean_text(self, text: str) -> str:
        """Clean extracted text"""
        # Remove excessive whitespace
        text = " ".join(text.split())
        
        # Fix common PDF extraction issues
        text = text.replace("ﬁ", "fi")
        text = text.replace("ﬂ", "fl")
        
        return text

preprocessor=SmartPDFProcessor()
preprocessor

## Process a PDF if available
try:
    smart_chunks=preprocessor.process_pdf("data/pdf/attention.pdf")
    print(f"Processed into {len(smart_chunks)} smart chunks")

    # Show enhanced metadata
    if smart_chunks:
        print("\nSample chunk metadata:")
        for key, value in smart_chunks[0].metadata.items():
            print(f"  {key}: {value}")

except Exception as e:
    print(f"Processing error: {e}")

# Processed into 49 smart chunks
# Sample chunk metadata:
#  producer: pdfTeX-1.40.25
#  creator: LaTeX with hyperref
#  creationdate: 2024-04-10T21:11:43+00:00
#  author: 
#  keywords: 
#  moddate: 2024-04-10T21:11:43+00:00
#  ptex.fullbanner: This is pdfTeX, Version 3.141592653-2.6-1.40.25 (TeX Live 2023) kpathsea version 6.3.5
#  subject: 
#  title: 
#  trapped: /False
#  source: data/pdf/attention.pdf
#  total_pages: 15
#  page: 1
#  page_label: 1
#  chunk_method: smart_pdf_processor
#  char_count: 2857
 
            

```

