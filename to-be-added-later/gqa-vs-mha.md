# GQA vs MHA

| Aspect                               | Multi-Head Attention (MHA)                    | Grouped-Query Attention (GQA)                        |
| ------------------------------------ | --------------------------------------------- | ---------------------------------------------------- |
| **Query computation**                | Each head computes its own query vector       | Multiple heads share queries within groups           |
| **Keys and values**                  | Independent per head                          | Independent per head                                 |
| **Computational cost**               | Higher due to separate queries per head       | Lower due to shared queries                          |
| **Performance**                      | Full expressiveness, slightly higher accuracy | Slight drop in accuracy, retains most expressiveness |
| **Use case**                         | Standard in most transformer models           | Used in efficient LLMs like Mistral                  |
| **Inference speed**                  | Slower, more resource-intensive               | Faster, lower GPU memory consumption                 |
| **Efficiency vs accuracy trade-off** | Less efficient, higher accuracy               | Balances efficiency and performance well             |
