# 🟢 Stage 2 of Training GPT - Inputs

* Usually cross entropy loss is used

| Task                 | Example Input Sequence to GPT                                                        | Tokens Used for Loss Calculation        |
| -------------------- | ------------------------------------------------------------------------------------ | --------------------------------------- |
| Question Answering   | `Q: What is AI? A: Artificial Intelligence is the simulation of human intelligence.` | Only tokens **after** `A:` (the answer) |
| Language Translation | `Translate English to French: The cat is on the mat. Le chat est sur le tapis.`      | Only tokens of the **translated text**  |
| Text Generation      | `Once upon a time, in a faraway land...`                                             | All tokens except prompt (if any)       |
| Sentiment Analysis   | `Review: The movie was great! Sentiment: Positive`                                   | Only tokens of the **sentiment label**  |
