# Third Prototype (Abstractive - BART/T5)

* **Generates new sentences**, rephrases, merges, shortens → closer to how humans summarize
* Encoder–Decoder (like a seq2seq transformer)
* Its pre-trained, we not fine tuned coz it requires GPU
* **BART-large** → max **1024 tokens**
* **T5-base** → max **512 tokens**
* If your document > limit → you must **chunk** it
* Break text into overlapping **chunks of 400–800 tokens**
* Overlap 50–100 tokens → prevents losing context at chunk boundaries
* Summarize each chunk separately
* Then **combine all summaries** → final summary
* We used BERTScore for evaluation, we got about <mark style="color:purple;background-color:purple;">**0.4**</mark> score
* It used to take lot of time, so had to summarize and mail to the users
* We used UI using which user uploaded the document, node for API, mongoDB for storing document and summary
* We even considered to provide it as a service using mail, but it was not approved

**Drawback:**

* Each chunk is summarized independently. Final summary may miss cross-chunk relationships.
* Each chunk summary is generated separately → slightly different tone or wording.
* Even for sumarizing 500 token chunk, it used to take 20 secs, so we had to make the service in background mode
*

```python
from transformers import pipeline

# Load summarizer
summarizer = pipeline("summarization", model="facebook/bart-large-cnn")

# Long document (example)
document = """<your long text here>"""

# Function to chunk
def chunk_text(text, max_tokens=800, overlap=100):
    words = text.split()
    chunks = []
    start = 0
    while start < len(words):
        end = min(start + max_tokens, len(words))
        chunk = " ".join(words[start:end])
        chunks.append(chunk)
        start += max_tokens - overlap
    return chunks

# Split into chunks
chunks = chunk_text(document)

# Summarize each chunk
summaries = [summarizer(chunk, max_length=150, min_length=50, do_sample=False)[0]['summary_text']
             for chunk in chunks]

# Combine summaries
final_summary = " ".join(summaries)

print(final_summary)

```
