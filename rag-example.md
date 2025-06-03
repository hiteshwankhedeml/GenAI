# RAG Example

* Datasource: Webpage
* DataLoader: To read the data from source
  * There are different data loaders
* The data can be huge, so we divide into chunks (Splitting the data)
* We will convert this into vectors
* There are different embeddings
* We will store this vectors into vector store
* Whenever we give query the vector store, it should give us context based on similarity search
* This when given to LLM should get correct output
* RAG means we have different source and we are able to chat with it
* In the prompt, the context is coming from vector store similarity search - The information coming from vector search will be given to context and then output will be given
* User query ⇒ Prompt ⇒ Context will be coming from vector DB ⇒ LLM will answer based on context
* Context can be multiple lines of text data, so we need to use Document chain

```python
from langchain_community.document_loaders import WebBaseLoader

loader=WebBaseLoader("https://python.langchain.com/docs/tutorials/llm_chain/")

document=loader.load()
# This will have a single document

from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter=RecursiveCharacterTextSplitter(chunk_size=1000,chunk_overlap=200)
documents=text_splitter.split_documents(document)
# After chunking, we will get multiple documents

from langchain_openai import OpenAIEmbeddings
embeddings=OpenAIEmbeddings()

from langchain_community.vectorstores import FAISS

vectorstore=FAISS.from_documents(documents,embeddings)

query="This is a relatively simple LLM application "

result=vectorstore.similarity_search(query)
result[0].page_content
# "In this quickstart we'll show you how to build a simple LLM application 
# with LangChain. This application will translate text from English into another
# language. This is a relatively simple LLM application - it's just a single
# LLM call plus some prompting. Still, this is a great way to get started
# with LangChain - a lot of features can be built with just some prompting
# and an LLM call!\nAfter reading this tutorial, you'll have a high level
# overview of:"

from langchain_core.prompts import ChatPromptTemplate

prompt=ChatPromptTemplate.from_template(
    """
Answer the following question based only on the provided context:
<context>
{context}
</context>


"""
)
#ChatPromptTemplate(input_variables=['context'], input_types={}, 
# partial_variables={}, messages=[HumanMessagePromptTemplate(
# prompt=PromptTemplate(input_variables=['context'], input_types={},
# partial_variables={}, template='\nAnswer the following question 
# based only on the provided context:\n<context>\n{context}\n</context>\n\n\n'),
# additional_kwargs={})])

from langchain.chains.combine_documents import create_stuff_documents_chain
document_chain=create_stuff_documents_chain(llm,prompt)
document_chain

vectorstore.similarity_search("Note that ChatModels receive message objects as inpu")
# This will multiple documents 

# Here we are adding context manually
document_chain.invoke({
    "input":"Note that ChatModels receive message objects as input",
    "context":[Document(page_content="Note that ..<<enter text added manually>>")]
})
# 'What are some ways that LangChain supports chat model inputs?'

# We dont need to do manually, we can do it using chain

retriever=vectorstore.as_retriever()
from langchain.chains import create_retrieval_chain
retrieval_chain=create_retrieval_chain(retriever,document_chain)
retrieval_chain

result=retrieval_chain.invoke({"input":"Note that ChatModels receive message objects as input"})
result['answer']
```
