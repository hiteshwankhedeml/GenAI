# Query Resolver

**Requirement:**

* The Ops team gets lots of queries from users and superusers on mail
* It was needed that using GenAI, queries should be answered auto when mailed on particular mail id
* We had suggested chatbot but there were 2 problems
  * We not had resource for immediate reply
  * Users find it easy to mail queries rather than asking bot



**Data Gathering:**

* We were provided a dump of mails which Ops team sends and were asked that it should use this for RAG



**Data Ingestion:**

* Mails were downloaded and provided as .msg files
* Using extract-msg we were able to read the mails
* We created Documents using this
* Number of mails: 90 Days \* 15Team member \* 20 = 30000 mails&#x20;



**Data Cleaning:**

* However in lot of mails we found that solutions were not mentioned, solution used to be discussed with users over teams or phone, and in mails it only used to be mentioned that as discussed closing the query
* This we highlighted to project owner
* So it was asked to all the Ops members - To reply with proper steps and justifications to all the mails and then we were asked to wait for some time
* Even then most of the mails were junk, so it was asked to Ops team to prepare SOPs for each resolution and team lead will be responsible for this
* Decision was taken that instead of mails, we will train our RAG on this SOP pdf
* About 300 pdfs were created for this
* Document length -&#x20;



**Training the RAG:**

*





* No resolution found
* Model used
* Mails











