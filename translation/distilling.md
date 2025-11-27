# Distilling

* Distillation is done by first running the large teacher model on a big set of sentences.
* The teacher generates soft probabilities for every token (not just the final answer).
* A smaller student model is created with fewer layers/parameters.
* Both teacher and student get the same input sentence.
* The training objective forces the student to match the teacher’s probability distribution for each next token.
* This is done using a distillation loss (KL-divergence between teacher and student outputs).
* Initialize student model by reducing no. of encoder / decoders
* Copy encoder and decoder layer weights
