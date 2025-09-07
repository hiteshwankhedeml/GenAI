# Pydantic model basics

* We define a pydantic model class
  * We define all the fields and their types&#x20;
  * We can also define if any field is optional
  * For int field we can also specify its range
* If input json contains more fields than present in pydantic model, then it won't give error
* it handles data coercion as well
  * If a field has been defined as character in model , and if we pass it as int then it will give error
  * If a field has been defined as int in model, and if pass it is character then it wont give error

```python
# Import libraries needed for the lesson
from pydantic import BaseModel, ValidationError, EmailStr
import json

# Create a Pydantic model for validating user input
class UserInput(BaseModel):
    name: str
    email: EmailStr
    query: str
    
# Create a model instance
user_input = UserInput(
    name="Joe User", 
    email="joe.user@example.com", 
    query="I forgot my password."
)
print(user_input)
# name='Joe User' email='joe.user@example.com' query='I forgot my password.'

# Attempt to create another model instance with an invalid email
user_input = UserInput(
    name="Joe User", 
    email="not-an-email", 
    query="I forgot my password."
)
# ValidationError

# Define a function to handle user input validation safely
def validate_user_input(input_data):
    try:
        # Attempt to create a UserInput model instance from user input data
        user_input = UserInput(**input_data)
        print(f"✅ Valid user input created:")
        print(f"{user_input.model_dump_json(indent=2)}")
        return user_input
    except ValidationError as e:
        # Capture and display validation errors in a readable format
        print(f"❌ Validation error occurred:")
        for error in e.errors():
            print(f"  - {error['loc'][0]}: {error['msg']}")
        return None

# Create an instance of UserInput using validate_user_input() function
input_data = {
    "name": "Joe User", 
    "email": "joe.user@example.com",
    "query": "I forgot my password."
}

user_input = validate_user_input(input_data)

# Attempt to create an instance of UserInput with missing query field
input_data = {
    "name": "Joe User", 
    "email": "joe.user@example.com"
}

user_input = validate_user_input(input_data)
# ❌ Validation error occurred:
#  - query: Field required

# Import additional libraries for enhanced validation
from pydantic import Field
from typing import Optional
from datetime import date

# Define a new UserInput model with optional fields
class UserInput(BaseModel):
    name: str
    email: EmailStr
    query: str
    order_id: Optional[int] = Field(
        None,
        description="5-digit order number (cannot start with 0)",
        ge=10000,
        le=99999
    )
    purchase_date: Optional[date] = None

# Define a dictionary with required fields only
input_data = {
    "name": "Joe User",
    "email": "joe.user@example.com",
    "query": "I forgot my password."
}

# Validate the user input data
user_input = validate_user_input(input_data)

input_data = {
    "name": "Joe User",
    "email": "joe.user@example.com",
    "query": "I forgot my password."
}

# Validate the user input data
user_input = validate_user_input(input_data)
# ✅ Valid user input created:
#{
#  "name": "Joe User",
#  "email": "joe.user@example.com",
#  "query": "I forgot my password.",
#  "order_id": null,
#  "purchase_date": null
#}

{
    "name": "Joe User",
    "email": "joe.user@example.com",
    "query": "I bought a keyboard and mouse and was overcharged.",
    "order_id": 12345,
    "purchase_date": "2025-12-31"
}
'''

# Parse the JSON string into a Python dictionary
input_data = json.loads(json_data)

user_input = UserInput.model_validate_json(json_data)
print(user_input.model_dump_json(indent=2))



```
