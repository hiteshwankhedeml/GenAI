# Reciprocal Rank Fusion in Hybrid Search

* From dense vector we will get top K results... Doc1, Doc2.....
*   Using keyword search we might get top k documents

    <figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
* To calculate search
* Semantic search is based on cosine similarity
* c is constant here and depends on DB (Range 0 to 60)
* For every document we will calculate score based on semantic rank and keyword rank
* So based on 50% weightage Document 1 will be better
* We can change weightage also
