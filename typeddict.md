# 🟢 TypedDict

* It lets you define dictionaries with **fixed keys and specific value types**

```python
from typing import TypedDict

class PersonDict(TypedDict):
    name: str
    age: int
    is_employee: bool

# Example usage
person: PersonDict = {
    "name": "Hitesh",
    "age": 38,
    "is_employee": True
}

person["age"] = "thirty-eight"  # ❌ Type checker error
```
