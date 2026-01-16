# 🟢 Query Decomposition - Implementation

Steps:

* decomposition\_chain ⇒ chain with prompt to decompose the questions into 2 to 4 smaller sub questions
* qa\_chain ⇒ for answering the sub questions
* create a function which will take query
  * call decomposition chain
  * get sub questions from chain response
  * for each sub question, call the llm

```python
from langchain.chat_models import init_chat_model
from langchain.prompts import PromptTemplate
from langchain.document_loaders import TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_huggingface import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS
from langchain_core.output_parsers import StrOutputParser
from langchain.chains.combine_documents import create_stuff_documents_chain
from langchain_core.runnables import RunnableSequence

# Step 1: Load and embed the document
loader = TextLoader("langchain_crewai_dataset.txt")
docs = loader.load()

splitter = RecursiveCharacterTextSplitter(chunk_size=300, chunk_overlap=50)
chunks = splitter.split_documents(docs)

embedding = HuggingFaceEmbeddings(model_name="all-MiniLM-L6-v2")
vectorstore = FAISS.from_documents(chunks, embedding)
retriever = vectorstore.as_retriever(search_type="mmr", search_kwargs={"k": 4, "lambda_mult": 0.7})

import os
from dotenv import load_dotenv
load_dotenv()

os.environ["GROQ_API_KEY"]=os.getenv("GROQ_API_KEY")

llm=init_chat_model(model="groq:gemma2-9b-it")
llm

# Step 3: Query decomposition
decomposition_prompt = PromptTemplate.from_template("""
You are an AI assistant. Decompose the following complex question into 2 to 4 smaller sub-questions for better document retrieval.

Question: "{question}"

Sub-questions:
""")
decomposition_chain = decomposition_prompt | llm | StrOutputParser()

query = "How does LangChain use memory and agents compared to CrewAI?"
decomposition_question=decomposition_chain.invoke({"question": query})

print(decomposition_question)
# Here's a breakdown of the complex question into smaller sub-questions:
# 1. **What types of memory mechanisms does LangChain offer for its models?**  (This focuses on LangChain's specific memory capabilities)
# 2. **How do LangChain agents utilize memory in their decision-making processes?** (This delves into the practical application of memory within LangChain agents)
# 3. **What memory and retrieval strategies does CrewAI employ for its AI agents?** (This shifts the focus to CrewAI's approach to memory)
# 4. **How do the memory and agent functionalities of LangChain and CrewAI differ in terms of implementation and capabilities?** (This encourages a comparative analysis) 

# Step 4: QA chain per sub-question
qa_prompt = PromptTemplate.from_template("""
Use the context below to answer the question.

Context:
{context}

Question: {input}
""")
qa_chain = create_stuff_documents_chain(llm=llm, prompt=qa_prompt)

# Step 5: Full RAG pipeline logic
def full_query_decomposition_rag_pipeline(user_query):
    # Decompose the query
    sub_qs_text = decomposition_chain.invoke({"question": user_query})
    sub_questions = [q.strip("-•1234567890. ").strip() for q in sub_qs_text.split("\n") if q.strip()]
    
    results = []
    for subq in sub_questions:
        docs = retriever.invoke(subq)
        result = qa_chain.invoke({"input": subq, "context": docs})
        results.append(f"Q: {subq}\nA: {result}")
    
    return "\n\n".join(results)

# Step 6: Run
query = "How does LangChain use memory and agents compared to CrewAI?"
final_answer = full_query_decomposition_rag_pipeline(query)
print("✅ Final Answer:\n")
print(final_answer)

```
