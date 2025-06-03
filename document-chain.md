# Document Chain

* [https://api.python.langchain.com/en/latest/chains/langchain.chains.combine\_documents.stuff.create\_stuff\_documents\_chain.html](https://api.python.langchain.com/en/latest/chains/langchain.chains.combine_documents.stuff.create_stuff_documents_chain.html)
* <mark style="color:purple;background-color:purple;">**Retriever queries the VectorDB to get the most relevant documents for your query.**</mark>
* <mark style="color:purple;background-color:purple;">**These retrieved documents are then passed to a Document Chain.**</mark>
* <mark style="color:purple;background-color:purple;">**The Document Chain feeds the documents (usually chunk by chunk) along with the query into the LLM.**</mark>
* <mark style="color:purple;background-color:purple;">**The LLM processes these inputs and generates the final answer or output.**</mark>
* Create stuff document chain creates a chain for passing a list of document to a model
* System ⇒ Message to the LLM ⇒ Instruction to LLM
* It will be using Chapprompt template, ChatOpenAI and StrOutputParser inside
* Here since we using the function create stuff document so we don't need | symbol
* Here we get a runnable binding ⇒ this allows us to pass documents on the run
*

```python
# pip install -U langchain langchain-community

from langchain_community.chat_models import ChatOpenAI
from langchain_core.documents import Document
from langchain_core.prompts import ChatPromptTemplate
from langchain.chains.combine_documents import create_stuff_documents_chain

prompt = ChatPromptTemplate.from_messages(
    [("system", "What are everyone's favorite colors:\n\n{context}")]
)
llm = ChatOpenAI(model="gpt-3.5-turbo")
chain = create_stuff_documents_chain(llm, prompt)

docs = [
    Document(page_content="Jesse loves red but not yellow"),
    Document(page_content = "Jamal loves green but not as much as he loves orange")
]

chain.invoke({"context": docs})
```
