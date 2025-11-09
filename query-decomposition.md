# Query Decomposition

* Taking a complex multi-part questions and breaking it into simpler, atomic sub-questions that can each be retrieved and answered individually
* Sub queries will be formed using LLM + Prompt
* Responses of all the queries will be combined using synthesizer and final answer will be formed
* But this involves lots of LLM calls

Why to use?

* Complex queries often involve multiple concepts
* LLMs or retrievers may miss parts of the original questions
* It enables multi-hop reasoning
* Allows parallelism



*

    <figure><img src=".gitbook/assets/{09C64DF1-D897-42C8-A2BE-617C90DF73C4}.png" alt=""><figcaption></figcaption></figure>
