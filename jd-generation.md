# JD Generation

**User Input:**

* Structured
  * Education Level
  * Experience
  * Function Area
  * Skills
* Unstructured
  * Existing JD
* Additional Instructions - For Fine Tuning
* Temperature



**List of Prompts:**

1. JD Generation ⇒ Based on inputs, temperature and additional instructions initial JD will be created
2. Inputs Consolidation ⇒ After every 5 inputs we will consolidate the inputs
3. JD  ⇒ If only additional instruction is given, then we refine the JD, we pass generated JD and instructions



**Generation:**

* Based on User Input ⇒ JD will be created
* If user changes the temperature ⇒ JD will be regenerated
* If user changes any of the inputs ⇒ JD will be regenerated
* User can can also give additional input ⇒ Based on this JD will be further fine tuned
* We are maintaining the versioning in mongo, there is a wrapper on node on top of this
* UI has been created in Angular/React



**Structured Output:**

* &#x20;Using pydantic



**Guardrails:**

* To save the API calls, before the JD is submitted for approval, we checked using openai ⇒ moderations
* We used the model - omni-moderation-latest

```python
from openai import OpenAI
client = OpenAI()

response = client.moderations.create(
    model="omni-moderation-latest",
    input="...text to classify goes here...",
)

print(response)
```



**Additional Feedback:**

* We have received from users that they dont want JD to be generated everytime, sometimes they might just want to ask LLM something&#x20;

