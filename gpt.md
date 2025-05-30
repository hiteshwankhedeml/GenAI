# GPT

* GPT Models ⇒ 3, 3,5, 4, 4V, 4o, mini
* Closed source model
* Unidirectional attention only
* Here we use auto regressive modelling
* They went to reddit, stackoverflow ⇒ where one is asking question and other is answering
* So they have taken such conversational data and fine tuned the base model ⇒ This is known as supervised fine tuning
* We will also do reinforcement learning on top of this
* We train model in 2 stages
* Unsupervised pretraining
  * We do this in Autoregressive modelling (Language Modelling)
  * Language modelling is also known as next word prediction
  * We try to predict the next word
  * Autoregressive means based on previous, we try to predict next
  * A                                          large
  * A large                                language
  * A large language              model
* Supervised Finetuning
