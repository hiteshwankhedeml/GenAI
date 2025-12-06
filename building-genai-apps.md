# 🟢 Building GENAI Apps

* <mark style="color:purple;background-color:purple;">**For all modern OpenAI models (GPT-4o, GPT-4o-mini, GPT-4-turbo, GPT-3.5-turbo), you must use ChatPromptTemplate.**</mark>
* <mark style="color:purple;background-color:purple;">**These models are chat models, so they require message-based prompts.**</mark>
* We have a website and it has some content, we will extract that information
* We will divide the contents into chunks, then embeddings
* Document chain is runnable binding
* Retriver can be considered to be an interface and its responsibility to get the data from vector store DB
* We convert vectorestoreDB into retriever
* <mark style="color:purple;background-color:purple;">**`ChatPromptTemplate.from_template`**</mark>
  * <mark style="color:purple;background-color:purple;">**Used to create a chat prompt from a single block of user content (implicitly as a human message).**</mark>
  * <mark style="color:purple;background-color:purple;">**Simpler and faster for one-shot user input, but doesn't support multiple roles.**</mark>
  * <mark style="color:purple;background-color:purple;">**Treats the entire content as a single human message**</mark>
*   <mark style="color:purple;background-color:purple;">**The query (**</mark><mark style="color:purple;background-color:purple;">**`input`**</mark><mark style="color:purple;background-color:purple;">**) is used only by the retriever to fetch relevant chunks.**</mark>

    <mark style="color:purple;background-color:purple;">**The LLM sees only**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`{context}`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**(the retrieved chunks).**</mark>
* <mark style="color:purple;background-color:purple;">**Since all retrieved chunks contain the answer in textual form, the LLM simply restates or summarizes the content.**</mark>
* <mark style="color:purple;background-color:purple;">**`input`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**→ required by retrieval chain (for search).**</mark>
* <mark style="color:purple;background-color:purple;">**`context`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**→ required by document chain (for stuffing into prompt).**</mark>
* <mark style="color:purple;background-color:purple;">**Only**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`context`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**must match an actual placeholder**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`{context}`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**in your prompt.**</mark>

<mark style="color:red;background-color:red;">**Steps:**</mark>

* <mark style="color:red;background-color:purple;">**Doc Loader**</mark>
* <mark style="color:red;background-color:purple;">**Chunk**</mark>
* <mark style="color:red;background-color:purple;">**Embeddings**</mark>
* <mark style="color:red;background-color:purple;">**vectordb(documents, embeddings)**</mark>
* <mark style="color:red;background-color:purple;">**Retriever = vectordb.as\_retriever()**</mark>
* <mark style="color:red;background-color:purple;">**document\_chain = create\_stuff\_document\_chain(llm, prompt)**</mark>
* <mark style="color:red;background-color:purple;">**retrieval\_chain = create\_retrieval\_chain(retriever, document\_chain)**</mark>
* <mark style="color:red;background-color:purple;">**retrieval\_chain.invoke()**</mark>

```python
import os
from dotenv import load_dotenv
load_dotenv()

os.environ['OPENAI_API_KEY']=os.getenv("OPENAI_API_KEY")
## Langsmith Tracking
os.environ["LANGCHAIN_API_KEY"]=os.getenv("LANGCHAIN_API_KEY")
os.environ["LANGCHAIN_TRACING_V2"]="true"
os.environ["LANGCHAIN_PROJECT"]=os.getenv("LANGCHAIN_PROJECT")

from langchain_community.document_loaders import WebBaseLoader

loader=WebBaseLoader("https://docs.smith.langchain.com/tutorials/Administrators/manage_spend")

docs=loader.load()

from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter=RecursiveCharacterTextSplitter(chunk_size=1000,chunk_overlap=200)
documents=text_splitter.split_documents(docs)

from langchain_openai import OpenAIEmbeddings
embeddings=OpenAIEmbeddings()

from langchain_community.vectorstores import FAISS
vectorstoredb=FAISS.from_documents(documents,embeddings)

## Query From a vector db
query="LangSmith has two usage limits: total traces and extended"
result=vectorstoredb.similarity_search(query)
result[0].page_content

from langchain_openai import ChatOpenAI
llm=ChatOpenAI(model="gpt-4o")

## Retrieval Chain, Document chain

from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain_core.prompts import ChatPromptTemplate

prompt=ChatPromptTemplate.from_template(
    """
Answer the following question based only on the provided context:
<context>
{context}
</context>


"""
)

document_chain=create_stuff_documents_chain(llm,prompt)

from langchain_core.documents import Document
document_chain.invoke({
    "input":"LangSmith has two usage limits: total traces and extended",
    "context":[Document(page_content="LangSmith has two usage limits: total traces and extended traces. These correspond to the two metrics we've been tracking on our usage graph. ")]
})
# 'LangSmith has two usage limits: total traces and extended traces. These correspond to the two metrics tracked on their usage graph.'

# We want the documents to come from retrieval
retriever=vectorstoredb.as_retriever()
from langchain.chains import create_retrieval_chain
retrieval_chain=create_retrieval_chain(retriever,document_chain)

## Get the response form the LLM
response=retrieval_chain.invoke({"input":"LangSmith has two usage limits: total traces and extended"})
response['answer']

response['context'] # To see the context

```
