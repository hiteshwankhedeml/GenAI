# 🟢 Evaluate RAG

* What RAGAS evaluates
  * RAGAS evaluates a RAG pipeline
  * Checks retrieval quality and answer quality
* <mark style="color:$danger;background-color:purple;">**What inputs RAGAS takes**</mark>
  * <mark style="color:$danger;background-color:purple;">**Question**</mark>
  * <mark style="color:$danger;background-color:purple;">**Retrieved context chunks**</mark>
  * <mark style="color:$danger;background-color:purple;">**Generated answer**</mark>
  * <mark style="color:$danger;background-color:purple;">**Optional ground-truth answer**</mark>
* How evaluation happens (simple flow)
  * <mark style="color:$danger;background-color:purple;">**Split the generated answer into small factual claims**</mark>
  * <mark style="color:$danger;background-color:purple;">**For each claim:**</mark>
    * <mark style="color:$danger;background-color:purple;">**Check if it is supported by retrieved context**</mark>
  * <mark style="color:$danger;background-color:purple;">**Use an LLM to score each check**</mark>
  * <mark style="color:$danger;background-color:purple;">**Aggregate scores into metrics**</mark>
* Key metrics and how they are calculated
  * <mark style="color:purple;background-color:purple;">**Faithfulness**</mark>
    * <mark style="color:purple;background-color:purple;">**Percentage of answer claims supported by context**</mark>
    * <mark style="color:purple;background-color:purple;">**Detects hallucinations**</mark>
  * Answer Relevance
    * <mark style="color:purple;background-color:purple;">**How well the answer addresses the question**</mark>
  * Context Precision
    * <mark style="color:purple;background-color:purple;">**How much of retrieved context was actually useful**</mark>
  * Context Recall
    * Whether important information was missing from retrieved context
* What the scores mean
  * Score range: 0 to 1
  * Higher score = better RAG behavior
  * Low score tells _where_ the pipeline failed
* Failure diagnosis using RAGAS
  * Low context recall → retrieval problem
  * Low context precision → noisy chunks
  * High retrieval scores + low faithfulness → hallucination
  * Low answer relevance → prompt or generation issue
* One-line summary
  * <mark style="color:purple;background-color:purple;">**RAGAS uses an LLM to check if the retrieved context was useful and if the answer was fully grounded in that context.**</mark>
