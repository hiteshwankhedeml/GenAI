# 🟢 Using LLMs

* <mark style="color:purple;background-color:purple;">**We did PoC using ollama and openai**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**Initially we did not get approval for open ai as resumes containing PII data**</mark>
* <mark style="color:purple;background-color:purple;">**Later we did PII masking and got approval**</mark>

```python
from langchain.document_loaders import PyPDFLoader
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate
from langchain.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field, validator
from typing import List

class ResumeSummary(BaseModel):
    """Resume summary with exactly 10 bullet points"""
    bullet_points: List[str] = Field(
        ...,
        description="Exactly 10 bullet points summarizing the resume",
        min_items=10,
        max_items=10
    )
    
    @validator('bullet_points')
    def validate_bullet_count(cls, v):
        if len(v) != 10:
            raise ValueError(f"Must have exactly 10 bullet points, got {len(v)}")
        return v

def summarize_resume_structured(documents, api_key=None):
    # Use ChatOpenAI for better structured output support
    llm = ChatOpenAI(temperature=0, openai_api_key=api_key, model="gpt-3.5-turbo")
    
    # Create output parser
    output_parser = PydanticOutputParser(pydantic_object=ResumeSummary)
    
    # Create prompt
    prompt = ChatPromptTemplate.from_messages([
        ("system", "You are an expert at analyzing resumes and extracting key information."),
        ("human", """
        Analyze the following resume and extract exactly 10 bullet points.
        
        Resume Text:
        {resume_text}
        
        {format_instructions}
        """)
    ])
    
    # Combine document content
    full_text = "\n\n".join([doc.page_content for doc in documents])
    
    # Format prompt with instructions
    formatted_prompt = prompt.format_prompt(
        resume_text=full_text,
        format_instructions=output_parser.get_format_instructions()
    )
    
    # Get response
    response = llm(formatted_prompt.to_messages())
    
    # Parse output
    summary = output_parser.parse(response.content)
    
    return summary
```
