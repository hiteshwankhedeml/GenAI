# 🟢 Knowledge Graph

* <mark style="color:purple;background-color:purple;">**It is a way of storing knowledge, but structured as entities and relationships, not plain text**</mark>
* <mark style="color:purple;background-color:purple;">**As a GenAI developer, it improves answer accuracy because relationships are explicit, not inferred**</mark>
* <mark style="color:purple;background-color:purple;">**It reduces hallucination since the model can query facts + links, not guess from text**</mark>
* It enables better RAG, where retrieval can follow relationships, not just similarity
* It helps in multi-hop questions like “who reports to whom” or “impact of X on Y”
* It allows explainable answers because you can trace the relationship path
* On SAP, this is implemented using SAP HANA Cloud graph + vector features
* Practically, you query graphs for structure and vectors for semantics, then combine both in GenAI
* Net impact: more reliable, enterprise-grade GenAI instead of chatty guesswork
