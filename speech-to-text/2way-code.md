# 2way - Code

```python
import requests
import base64
import json


# -------------------------
# CONFIG
# -------------------------
VLLM_URL = "http://<VM-IP>:8000/v1/chat/completions"
MODEL = "meta-llama/Meta-Llama-3-8B-Instruct"


# -------------------------
# 1. SPEECH → TEXT
# -------------------------
def speech_to_text(audio_base64: str) -> str:
    """
    Replace with Whisper API or on-prem model call.
    For now mock a transcript.
    """
    # If STT fails return None or empty string
    transcript = "mock transcript"  
    return transcript


# -------------------------
# 2. EVALUATE CURRENT ANSWER
# -------------------------
def evaluate_answer(question: str, answer: str):
    """
    LLM returns relevance + score
    """
    prompt = f"""
You are an evaluator. Check if the answer is relevant to the question.
Return JSON only with keys: relevance (true/false), score (0-10).

Question: {question}
Answer: {answer}
"""
    payload = {
        "model": MODEL,
        "messages": [{"role": "user", "content": prompt}],
        "temperature": 0
    }

    resp = requests.post(VLLM_URL, json=payload)
    content = resp.json()["choices"][0]["message"]["content"]

    # extract json
    js = json.loads(content)
    return js["relevance"], js["score"]


# -------------------------
# 3. CHECK NEXT QUESTION RELEVANCE
# -------------------------
def check_next_question_relevance(candidate_question: str, overall_conv: list):
    """
    Pass question + conversation to LLM.
    LLM decides if the next question is relevant.
    """
    conv_text = ""
    for item in overall_conv:
        conv_text += f"Q: {item['question']}\nA: {item['answer']}\n"

    prompt = f"""
Based on the interview so far, check if the following next question
should be asked.

Conversation so far:
{conv_text}

Next Candidate Question: {candidate_question}

Return JSON: {{ "relevant": true/false }}
"""

    payload = {
        "model": MODEL,
        "messages": [{"role": "user", "content": prompt}],
        "temperature": 0
    }

    resp = requests.post(VLLM_URL, json=payload)
    content = resp.json()["choices"][0]["message"]["content"]
    js = json.loads(content)
    return js["relevant"]


# -------------------------
# 4. MAIN LOGIC FUNCTION
# -------------------------
def process_step(input_json):
    avatar = input_json["avatar_details"]
    current_question = input_json["question"]
    question_set = input_json["question_set"]
    overall_conv = input_json["overall_conv"]
    audio_response = input_json["response"]

    # STEP 1 — STT
    transcript = speech_to_text(audio_response)
    if not transcript:
        return {
            "next_question": current_question,
            "response": "",
            "overall_conv": overall_conv,
            "response_speech": "",
            "repeat": True
        }

    # STEP 2 — LLM evaluation
    relevance, score = evaluate_answer(current_question, transcript)

    if not relevance:
        return {
            "next_question": current_question,
            "response": transcript,
            "overall_conv": overall_conv,
            "response_speech": "",
            "repeat": True
        }

    # relevance = true → update conversation
    overall_conv.append({"question": current_question, "answer": transcript})

    # STEP 3 — get next question
    current_index = question_set.index(current_question)
    remaining_questions = question_set[current_index + 1:]

    next_question = None
    for q in remaining_questions:
        is_relevant = check_next_question_relevance(q, overall_conv)
        if is_relevant:
            next_question = q
            break

    # STEP 4 — handle end of interview
    if next_question is None:
        return {
            "next_question": None,
            "response": transcript,
            "overall_conv": overall_conv,
            "response_speech": "",
            "repeat": False
        }

    # convert next question to "speech" (mock base64)
    response_speech = base64.b64encode(next_question.encode()).decode()

    return {
        "next_question": next_question,
        "response": transcript,
        "overall_conv": overall_conv,
        "response_speech": response_speech,
        "repeat": False
    }


# -------------------------
# EXAMPLE
# -------------------------
input_json = {
    "avatar_details": {},
    "question": "Tell me about your current job role",
    "question_set": [
        "Tell me about your current job role",
        "Explain your major achievements",
        "Describe your sales experience",
        "Explain how you handled a difficult customer"
    ],
    "overall_conv": [],
    "response": "<base64_audio_here>"
}

output = process_step(input_json)
print(json.dumps(output, indent=2))

```
