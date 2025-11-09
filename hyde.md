# HyDE

* Hypothetical document embedding
* Instead of embedding the users query directly, you first generate a hypothetical answer to the query using an LLM, and then embed that hypothetical document to search your vector store
* Used when?
  * Queries are short
  * Language mismatch
  * Retrieve based on answer content and not on query
*

    <figure><img src=".gitbook/assets/{C5CB4C9E-E499-4256-AB71-ECFC4DBD6A88}.png" alt=""><figcaption></figcaption></figure>
