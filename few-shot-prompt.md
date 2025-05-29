# 🟢 Few- Shot Prompt

* <mark style="color:purple;background-color:purple;">**Few-shot prompting involves giving a few input-output examples to the language model inside the prompt.**</mark>
* <mark style="color:purple;background-color:purple;">**The model learns the pattern from these examples and completes a new input accordingly.**</mark>
* Useful for **translation, classification, extraction, generation, etc.**

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# 1. Few-shot examples
few_shot_examples = """
Example 1:
English: Where is the nearest restaurant?
French: Où est le restaurant le plus proche ?

Example 2:
English: I would like a coffee without sugar.
French: Je voudrais un café sans sucre.
"""

# 2. Create a prompt template
prompt_template = PromptTemplate(
    input_variables=["input_sentence"],
    template=f"""Translate the following English sentence to French.

{few_shot_examples}

English: {{"{{input_sentence}}"}}  
French:"""
)

# 3. Load the LLM
llm = ChatOpenAI(temperature=0)  # Use OpenAI() if using older model

# 4. Create the chain
chain = LLMChain(llm=llm, prompt=prompt_template)

# 5. Run the chain
output = chain.run("Can you help me with my luggage?")
print(output)

```
