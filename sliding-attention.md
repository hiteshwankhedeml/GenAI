# Sliding Attention

* Sliding window attention splits long input sequences into overlapping chunks (windows).
* Each chunk attends only to tokens within its window, not the entire sequence.
* Overlapping windows ensure context continuity between chunks.
* This reduces memory and compute costs compared to full attention over the entire sequence.
* Useful for processing very long texts that exceed model’s maximum context length.
* Enables efficient streaming or incremental processing of data.
* Trade-off: may lose some long-range dependencies beyond window size.
* Used in models like Mistral to handle 8K+ token contexts efficiently.
