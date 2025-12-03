# 2 way - Interview Questions

* Extract exactly 4 evaluation areas (competencies) from the job description.
* For each area, generate 2–3 main questions.
* Questions must be ordered inside each area from basic → deeper.
* Areas must also be in a logical sequence based on the job role.
* Questions must be clear, role-specific, and non-generic.
* No random generation. Follow the structure strictly.
* Return output ONLY in JSON.

Output:

Area1:

* Q1
* Q2
* Q3

Area2 ....

* Interviewer can also add/remove Areas/Questions
* We create API
  * Input:
    * No. of Areas
    * No. of Questions/Area
    * JD
  * Output:
  * json with Area and Questions
* Calling application stored this data into mongo — not our responsibility

```python
import os
import requests
import json

# -------------------------
# vLLM (on-prem) config
# -------------------------
VLLM_BASE_URL = "http://<VM-IP>:8000/v1/chat/completions"
MODEL = "meta-llama/Meta-Llama-3-8B-Instruct"

# -------------------------
# Prompt Builder
# -------------------------
def build_question_gen_prompt(jd: str):
    return f"""
You are an expert interview designer.

Read the job description below and generate structured interview questions.

Rules:
- Extract exactly 4 evaluation areas (competencies).
- For each area, generate 2–3 questions.
- Questions must be ordered from basic → deeper.
- Areas must also be in logical sequence.
- Output ONLY valid JSON.

JSON Format:
{{
  "areas": [
    {{
      "area_name": "string",
      "questions": [
        {{
          "question_id": "A1-Q1",
          "question_text": "string",
          "purpose": "what this question evaluates"
        }}
      ]
    }}
  ]
}}

Job Description:
\"\"\"
{jd}
\"\"\"
"""


# -------------------------
# LLM Call
# -------------------------
def generate_interview_questions(jd: str):
    prompt = build_question_gen_prompt(jd)

    payload = {
        "model": MODEL,
        "messages": [
            {"role": "system", "content": "You generate structured interview questions."},
            {"role": "user", "content": prompt}
        ],
        "temperature": 0
    }

    resp = requests.post(VLLM_BASE_URL, json=payload)
    data = resp.json()

    raw_output = data["choices"][0]["message"]["content"]

    # Extract JSON (in case LLM adds text)
    try:
        start = raw_output.index("{")
        end = raw_output.rindex("}") + 1
        json_text = raw_output[start:end]
    except:
        json_text = raw_output

    return json.loads(json_text)


# -------------------------
# Example
# -------------------------
if __name__ == "__main__":
    jd_text = """
    We are hiring a Sales Executive responsible for distributor management,
    territory sales planning, retailer onboarding, customer service, and daily reporting.
    Candidate must handle sales targets, achieve KPIs, build retailer relationships,
    coordinate with distributors, and drive local promotions.
    """

    result = generate_interview_questions(jd_text)
    print(json.dumps(result, indent=2))

```
