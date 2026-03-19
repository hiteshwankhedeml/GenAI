---
hidden: true
---

# ✈️ Chain of Thoughts

* <mark style="color:purple;background-color:purple;">**CoT prompting is a technique where you prompt a language model to reason step-by-step before giving the final answer.**</mark>
* This is done by adding a **simple cue phrase** like "Let's think step by step," which encourages the model to **break down the problem logically**.
* It helps improve reasoning on **complex, multi-step tasks** such as math problems, logic puzzles, or word problems.

***

#### 🔑 Key Idea

* Add a natural language prompt encouraging reasoning:
  * "Let's think step by step."
  * "Let's break this down."
  * "Here’s how to solve it:"
* <mark style="color:purple;background-color:purple;">**This cue leads the model to generate intermediate reasoning steps, not just a final answer.**</mark>

***

#### 🧪 LangChain Example: CoT Prompting for Math Problem

```python
from langchain.chat_models import ChatOpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# Prompt template with CoT cue
prompt_template = PromptTemplate(
    input_variables=["question"],
    template="""Answer the math problem with step-by-step reasoning.

Question: {{question}}
Answer: Let's think step by step."""
)

llm = ChatOpenAI(temperature=0)
chain = LLMChain(llm=llm, prompt=prompt_template)

output = chain.run("If a train travels at 60 km/h for 2 hours, how far does it go?")
print(output)
```

**Expected Output:**

```
Let's think step by step.  
The train travels at 60 km/h.  
It travels for 2 hours.  
Distance = speed × time = 60 × 2 = 120 km.  
So the train travels 120 km.
```

***

#### 🔍 Without vs With Chain of Thought Prompting

| Prompt Style  | Example Prompt                                                                | Result Style                         |
| ------------- | ----------------------------------------------------------------------------- | ------------------------------------ |
| ❌ Without CoT | “What’s 25 + 68?”                                                             | “93”                                 |
| ✅ With CoT    | “What’s 25 + 68? Let’s think step by step.”                                   | “25 + 68 = 93. So the answer is 93.” |
| ❌ Without CoT | “If a pen costs $2 and I buy 3 pens and a notebook for $5, what’s the total?” | “11”                                 |
| ✅ With CoT    | “Let’s solve this step by step.”                                              | “3 pens at $2 = $6. $6 + $5 = $11.”  |

***

#### 🔁 Summary: Why Use CoT?

* Enables the model to **show intermediate reasoning steps**.
* Improves accuracy on **multi-step problems**.
* Makes outputs more **interpretable and trustworthy**.
* Simple to implement by **adding a reasoning cue** in the prompt.
