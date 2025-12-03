# 2 way - Flow

* Input JSON looks like:\
  {\
  "avatar\_details": {},\
  "question": "string",\
  "question\_set": \["Q1", "Q2", "Q3", "Q4"],\
  "overall\_conv": \[ { "question": "Q1", "answer": "A1" } ],\
  "response": "\<audio\_base64>"\
  }
* Convert audio to text; if STT fails, set Repeat = true and return the same question.
* Pass the current question and transcript to the LLM to get relevance and score.
* If relevance is false, ask the same question again and set Repeat = true.
* If relevance is true, append the question and answer to overall\_conv.
* Loop through remaining questions (those after the current question).
* For each future question, send that question and overall\_conv to the LLM to check if it is relevant to ask next.
* The first question marked relevant becomes next\_question.
* If no future question is relevant, next\_question = null and the interview ends.
* If next\_question exists, convert it to base64 speech and set Repeat = false.
* Output JSON looks like:\
  {\
  "next\_question": "Q3",\
  "response": "transcribed text",\
  "overall\_conv": \[\
  { "question": "Q1", "answer": "A1" },\
  { "question": "Q2", "answer": "candidate\_answer" }\
  ],\
  "response\_speech": "\<base64\_audio>",\
  "repeat": false\
  }
