# Chunking

* Very important preprocessing technique
* We can store entire data in vectorDB without chunking
* We don't chunk so as not to lose the context
* But if data is too huge, then we will have to chunk the data
* Lets say we have a PDF data which has 100 pages consisting of text, images , tables
* Each PDF page will be document containing of metadata, page\_content, embedding
* This will be stored in vectorDB
* Table can be stored as json somewhere and UUID will be created
* Chunksize as well as overlap is hyper parameter&#x20;
* We use overlapping to sustain the context of given sentence



**When to use chunking:**

* The token size of the text exceeds the size supported by the model
* Handle non-uniform document lenght:
  * If pdf has 10 pages, and on each number of tokens is different
  * So when we read pdf and store in vectorDB then in each chunk the number of tokens will be different
  * So if we chunk then there will be uniform number of tokens
* For fast retrieval
  * If large chunk is used, then embedding will be large so to find the relevant documents it will take time
* If chunks will be small, processing will be minimized, so less computation resources will be needed
  * Computation for finding similarity will be less
* Improve the representation quality. if the chunk is large then entire document will be passed to model. If the chunk is smaller then only relevant document will be passed to the model

