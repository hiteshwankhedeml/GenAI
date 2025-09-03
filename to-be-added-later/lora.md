# LoRA

* Main technique inside PEFT
* Without modifying all the parameters, we train the model ⇒ As it reduces computational cost
* It uses less memory
* Speeds up the process
* Performance can be reduced, but we can maintain it and it won't reduce drastically



Llama ⇒ Trained on transformer architecture

* It has self attention and NN layers
* There are weights in both the layers
* We won't take entire weights, we will freeze layers
* We will add more layers, which we will be trained
* Let A and B be the new layers
* W is old weight, W' updated weights, d = dimension
* r = low rank factor
*

    <figure><img src="../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>
* Rank is calculated using algebra using SVD
* LoRA is not taking subset of the weights of the transformers
* We add additional weights
*

    <figure><img src="../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>
* In self attention, we have 3 matrix - Query, Key and Value
* Suppose we want to change on Qproj and Kproj
* So on top of this we will add extra weights
* During fine tuning, we will be changing extra weights only
* Initially weights will be random values
* Size of A and B depends on rank
* Which projection to change is hyper-parameter
*
*

    <figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>





LoRA = 32GB RAM then can be used

QLoRA = 12 to 24 GB RAM can be used
