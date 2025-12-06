# 🟢 Data Loader - MongoDB

<mark style="color:purple;background-color:purple;">**Steps:**</mark>

* <mark style="color:purple;background-color:purple;">While initiating loader , we need to specify credentials, connection string, filtering condition etc.</mark>
* <mark style="color:purple;background-color:purple;">loader.load()</mark>

```python
from langchain_community.document_loaders.mongodb import MongodbLoader

loader = MongodbLoader(
    connection_string="mongodb://localhost:27017/",
    db_name="sample_restaurants",
    collection_name="restaurants",
    filter_criteria={"borough": "Bronx", "cuisine": "Bakery"},
    field_names=["name", "address"],
)

docs = loader.load()

len(docs)
```
