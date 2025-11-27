# KL Divergence

* KL divergence measures **how far one probability distribution is from another**.
* Example: Teacher model predicts next word probabilities:
  * A = 0.7, B = 0.2, C = 0.1
* Student model predicts:
  * A = 0.5, B = 0.3, C = 0.2
* KL divergence will be computed as:
  * `0.7 * log(0.7/0.5) + 0.2 * log(0.2/0.3) + 0.1 * log(0.1/0.2)`
* If student probabilities are close to teacher, KL value is small.
* If student is very different, KL value becomes large.
