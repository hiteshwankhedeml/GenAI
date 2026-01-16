---
hidden: true
---

# ✈️ Validating LLM Responses

* &#x20;

````python
# Import necessary packages
from pydantic import BaseModel, ValidationError, Field, EmailStr
from typing import List, Literal, Optional
import json
from datetime import date
from dotenv import load_dotenv
import openai

user_input_json = '''
{
    "name": "Joe User",
    "email": "joe.user@example.com",
    "query": "I forgot my password.",
    "order_number": null,
    "purchase_date": null
}
'''

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

user_input = UserInput.model_validate_json(user_input_json)

# Define the CustomerQuery model that inherits from UserInput
class CustomerQuery(UserInput):
    priority: str = Field(
        ..., description="Priority level: low, medium, high"
    )
    category: Literal[
        'refund_request', 'information_request', 'other'
    ] = Field(..., description="Query category")
    is_complaint: bool = Field(
        ..., description="Whether this is a complaint"
    )
    tags: List[str] = Field(..., description="Relevant keyword tags")

# Create a prompt with generic example data to guide LLM.
example_response_structure = f"""{{
    name="Example User",
    email="user@example.com",
    query="I ordered a new computer monitor and it arrived with the screen cracked. I need to exchange it for a new one.",
    order_id=12345,
    purchase_date="2025-12-31",
    priority="medium",
    category="refund_request",
    is_complaint=True,
    tags=["monitor", "support", "exchange"] 
}}"""

# Create prompt with user data and expected JSON structure
prompt = f"""
Please analyze this user query\n {user_input.model_dump_json(indent=2)}:

Return your analysis as a JSON object matching this exact structure 
and data types:
{example_response_structure}

Respond ONLY with valid JSON. Do not include any explanations or 
other text or formatting before or after the JSON object.
"""

print(prompt)

# Define a function to call the LLM
def call_llm(prompt, model="gpt-4o"):
    response = client.chat.completions.create(
        model=model,
        messages=[{"role": "user", "content": prompt}]
    )
    return response.choices[0].message.content

# Get response from LLM
response_content = call_llm(prompt)
print(response_content)
# ```json
# {
#     "name": "Joe User",
#     "email": "joe.user@example.com",
#     "query": "I forgot my password.",
#     "order_id": null,
#     "purchase_date": null,
#     "priority": "low",
#     "category": "account_issue",
#     "is_complaint": false,
#     "tags": ["password", "account", "support"]
# }
# ```

valid_data = CustomerQuery.model_validate_json(response_content)
# ValidationError                           Traceback (most recent call last)

# Define a function to validate an LLM response
def validate_with_model(data_model, llm_response):
    try:
        validated_data = data_model.model_validate_json(llm_response)
        print("data validation successful!")
        print(validated_data.model_dump_json(indent=2))
        return validated_data, None
    except ValidationError as e:
        print(f"error validating data: {e}")
        error_message = (
            f"This response generated a validation error: {e}."
        )
        return None, error_message

# Define a function to create a retry prompt with error feedback
def create_retry_prompt(
    original_prompt, original_response, error_message
):
    retry_prompt = f"""
This is a request to fix an error in the structure of an llm_response.
Here is the original request:
<original_prompt>
{original_prompt}
</original_prompt>

Here is the original llm_response:
<llm_response>
{original_response}
</llm_response>

This response generated an error: 
<error_message>
{error_message}
</error_message>

Compare the error message and the llm_response and identify what 
needs to be fixed or removed
in the llm_response to resolve this error. 

Respond ONLY with valid JSON. Do not include any explanations or 
other text or formatting before or after the JSON string.
"""
    return retry_prompt

validation_retry_prompt = create_retry_prompt(
    original_prompt=prompt,
    original_response=response_content,
    error_message=validation_error
)

print(validation_retry_prompt)

validation_retry_response = call_llm(validation_retry_prompt)
print(validation_retry_response)

# Attempt to validate retry response from LLM
validated_data, validation_error = validate_with_model(
    CustomerQuery, validation_retry_response
)

# Create a second retry prompt for validation errors
second_validation_retry_prompt = create_retry_prompt(
    original_prompt=validation_retry_prompt,
    original_response=validation_retry_response,
    error_message=validation_error
)

print(second_validation_retry_prompt)

# Call the LLM with the second validation retry prompt
second_validation_retry_response = call_llm(
    second_validation_retry_prompt
)
print(second_validation_retry_response)

# Define a function to automatically retry an LLM call multiple times
def validate_llm_response(
    prompt, data_model, n_retry=5, model="gpt-4o"
):
    # Initial LLM call
    response_content = call_llm(prompt, model=model)
    current_prompt = prompt

    # Try to validate with the model
    # attempt: 0=initial, 1=first retry, ...
    for attempt in range(n_retry + 1):

        validated_data, validation_error = validate_with_model(
            data_model, response_content
        )

        if validation_error:
            if attempt < n_retry:
                print(f"retry {attempt} of {n_retry} failed, trying again...")
            else:
                print(f"Max retries reached. Last error: {validation_error}")
                return None, (
                    f"Max retries reached. Last error: {validation_error}"
                )

            validation_retry_prompt = create_retry_prompt(
                original_prompt=current_prompt,
                original_response=response_content,
                error_message=validation_error
            )
            response_content = call_llm(
                validation_retry_prompt, model=model
            )
            current_prompt = validation_retry_prompt
            continue

        # If you get here, both parsing and validation succeeded
        return validated_data, None

# Test your complete solution with the original prompt
validated_data, error = validate_llm_response(
    prompt, CustomerQuery
)


    






````
