# LLM Evaluate

* We first created a golden dataset by collecting 100–300 real question–answer pairs from past interviews.
* Each answer was scored by 2–3 human evaluators on a scale of 0–10 to establish reliable ground truth.
* We averaged the human scores to reduce individual bias and finalize the golden labels.
* We shortlisted multiple on-prem open-source LLMs such as Llama-3 8B, Mistral 7B, and Qwen2 7B for evaluation.
* We built a fixed grading prompt with clear instructions, few-shot examples, and enforced JSON output to ensure fair comparison across models.
* We ran each LLM on the entire golden dataset, generating a predicted score for every answer.
* We computed MAE to check how close the LLM’s numeric scores were to human scores on average.
* We found that MAE alone is insufficient, because a model that always outputs “5” can still have a reasonable MAE but provide no useful ranking.
* We calculated correlation (Pearson/Spearman) to measure whether the LLM’s scoring pattern aligns with human judgment across all answers.
* Correlation shows whether the model ranks good answers high and weak answers low, which is essential for interview scoring.
