# 🟢 Scoring - LLM - PoC

```python
from langchain_community.llms import Ollama

llm = Ollama(
    model="mistral",
    base_url="http://<VM_IP>:11434"
)
```



* If expected answers are available, we pass them to the LLM so it can compare the candidate's response with our reference answers.
* This makes the scoring objective, because the LLM knows exactly what a good answer should contain.
* If expected answers are not available, the LLM scores using only the question and candidate answer, but this is less reliable and more subjective.
* So the pipeline is designed such that expected answers are used whenever they exist, and the LLM uses them to generate a more accurate 0–10 score.

```python
# ---------------------------------------------------------
# LLM SCORING USING LANGCHAIN + VLLM + PYDANTIC
# ---------------------------------------------------------

from langchain_openai import ChatOpenAI
from langchain_core.prompts import PromptTemplate
from langchain_core.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field

# ---------- 1. Pydantic schema ----------
class ScoreResponse(BaseModel):
    score: float = Field(..., description="Score between 0 and 10")
    reason: str = Field(..., description="Short explanation of the score")

parser = PydanticOutputParser(pydantic_object=ScoreResponse)

# ---------- 2. LLM connection (on-prem vLLM) ----------
llm = ChatOpenAI(
    model="meta-llama/Meta-Llama-3-8B-Instruct",
    base_url="http://<VM-IP>:8000/v1",
    api_key="dummy",
    temperature=0
)

# ---------- 3. Prompt template ----------
template = """
You are an interview evaluator.

Your task:
- Compare the candidate's answer with expected answers.
- Score on a scale of 0 to 10.
- Reward clarity, correctness, completeness.
- Penalize short or incomplete answers.
- Penalize contradictions or negations.

Return only JSON in this structure:
{format_instructions}

Question: {question}
Expected Answers: {expected}
Candidate Answer: {answer}
"""

prompt = PromptTemplate(
    input_variables=["question", "expected", "answer"],
    partial_variables={"format_instructions": parser.get_format_instructions()},
    template=template
)

# ---------- 4. Build the scoring chain ----------
chain = prompt | llm | parser

# ---------- 5. Scoring function ----------
def llm_score(question, expected_answers, candidate_answer):
    return chain.invoke({
        "question": question,
        "expected": expected_answers,
        "answer": candidate_answer
    })

# ---------- 6. Example ----------
if __name__ == "__main__":
    question = "Explain your current job role"
    expected = [
        "I manage sales targets and handle distributor operations.",
        "I coordinate with customers and resolve issues.",
        "I ensure daily coverage and reporting."
    ]
    candidate = "I handle customer issues and manage daily sales targets."

    result = llm_score(question, expected, candidate)
    print(result)

```
