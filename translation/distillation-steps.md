# Distillation Steps

* **Get batch of parallel data** (src, tgt).
* **Run teacher forward pass** → get teacher logits → convert to soft probabilities with temperature.
* **Run student forward pass** → get student logits → convert to log-soft probabilities.
* **Compute KL divergence** between teacher probabilities and student probabilities.
* **Optionally compute CE loss** against the true target tokens.
* **Combine losses** (e.g., 70% KL + 30% CE).
* **Backprop + optimize** student weights.
