---
hidden: true
---

# ✈️ Document Chains

* <mark style="color:purple;background-color:purple;">**Document Chains are strategies for combining docs before sending to LLM.**</mark>
* `create_stuff_documents_chain` is used in RAG pipelines.
* <mark style="color:purple;background-color:purple;">**It follows the stuffing approach → concatenates all retrieved docs into a single prompt.**</mark>
* Best when the number of docs is small and fits in the context window.
* <mark style="color:purple;background-color:purple;">**Workflow: retrieve docs → concatenate → insert into prompt → pass to LLM → get answer.**</mark>
* <mark style="color:purple;background-color:purple;">**Advantages: simple, LLM sees all info at once.**</mark>
* <mark style="color:purple;background-color:purple;">**Disadvantages: not scalable for large/many docs.**</mark>
* <mark style="color:purple;background-color:purple;">**Main types of Document Chains:**</mark>
  * <mark style="color:purple;background-color:purple;">**Stuff Chain: all docs stuffed into prompt at once.**</mark>
  * <mark style="color:purple;background-color:purple;">**Map-Reduce Chain: process each doc individually (map) → combine results (reduce).**</mark>
  * <mark style="color:purple;background-color:purple;">**Refine Chain: generate answer from one doc → iteratively refine with additional docs.**</mark>
* Rule of thumb: Small dataset → Stuff, Large dataset → Map-Reduce, Progressive improvement → Refine.
