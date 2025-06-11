# ✈️ Text Splitting - Recursive json splitter

* This json splitter splits json data while allowing control over chunk sizes. It traverses json data depth first and builds smaller json chunks. It attempts to keep nested json objects whole but will split them if needed to keep chunks between a min\_chunk\_size and the max\_chunk\_size.
* If the value is not a nested json, but rather a very large string the string will not be split. If you need a hard cap on the chunk size consider composing this with a Recursive Text splitter on those chunks.
* There is an optional pre-processing step to split lists, by first converting them to json (dict) and then splitting them as such.
* How the text is split: json value.
* How the chunk size is measured: by number of characters

```python
import json
import requests

json_data=requests.get("https://api.smith.langchain.com/openapi.json").json()

from langchain_text_splitters import RecursiveJsonSplitter
json_splitter=RecursiveJsonSplitter(max_chunk_size=300)
json_chunks=json_splitter.split_json(json_data)


```
