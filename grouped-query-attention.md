# Grouped Query Attention

* GQA is used in Mistral
* GQA is an efficient variant of Multi-Head Attention (MHA).
* Instead of each head having its own query, multiple heads share queries within groups.
* Keys and values remain independent per head, preserving diverse attention.
* This reduces computation and memory usage while retaining most expressiveness of full MHA.
* GQA enables faster inference and reduced GPU memory use compared to standard MHA.
* Especially useful in large transformer models where attention cost is high.
* GQA improves efficiency by sharing queries among heads, reducing computation and memory use.
* It preserves performance by keeping keys and values independent, allowing diverse attention.
* This trade-off lowers cost with minimal accuracy loss, making it ideal for large models like Mistral.
