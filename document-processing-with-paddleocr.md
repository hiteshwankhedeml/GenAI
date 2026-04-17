# 🟢 Document Processing with PaddleOCR

* <mark style="color:$danger;background-color:purple;">**Here we created a tool for ocr**</mark>
* <mark style="color:$danger;background-color:purple;">**Then created agent and have it this tool**</mark>
* <mark style="color:$danger;background-color:purple;">**Invoke the agent by passing the document**</mark>
* Handwriting remained challenging
* **\_DET**: Text detection model (locates text regions)
* **\_REC**: Text recognition model (reads characters)
* PaddleOCR preprocesses images automatically—correcting rotation, deskewing, and removing noise.
* rec\_texts ⇒ contains text
* rec\_scores ⇒ contains confidence scores
* rec\_poly ⇒ contains bounding boxes

```python
import os
from pathlib import Path
from typing import List, Dict, Any

import numpy as np
import matplotlib.pyplot as plt
from matplotlib import colormaps

from langchain_core.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain.agents import create_tool_calling_agent
from langchain.agents import AgentExecutor

from langchain_openai import ChatOpenAI

from PIL import Image
import cv2

os.environ['DISABLE_MODEL_SOURCE_CHECK'] = 'True'
from paddleocr import PaddleOCR

from dotenv import load_dotenv

# Load environment variables from .env
_ = load_dotenv(override=True)

# Initialize English OCR model
ocr = PaddleOCR(lang='en')

image_path = 'receipt.jpg'
img = Image.open(image_path)
display(img)

result = ocr.predict(image_path)

page = result[0]
texts = page['rec_texts'] # recognized text strings
scores = page['rec_scores'] # confidence scores for each text line
boxes  = page['rec_polys'] #  bounding box coordinates

for text, score, box in zip(texts, scores, boxes):
    # box is a numpy array like [[x1, y1], [x2, y2], [x3, y3], [x4, y4]]
    coords = box.astype(int).tolist()  # convert to normal list of ints
    print(f"{text:25} | {score:.3f} | {coords}")

from langchain.tools import tool

@tool
def paddle_ocr_read_document(image_path: str) -> List[Dict[str, Any]]:
    """
    Reads an image from the given path and returns extracted text 
    with bounding boxes.
    
    Returns a list of dictionaries, each containing:
    - 'text': the recognized text string
    - 'bbox': bounding box coordinates [x_min, y_min, x_max, y_max]
    - 'confidence': recognition confidence score (if available)
    """
    try:
        result = ocr.predict(image_path)
        page = result[0]
        
        texts = page['rec_texts'] 
        boxes = page['dt_polys']         
        scores = page.get('rec_scores', [None] * len(texts))  
        
        extracted_items = []
        for text, box, score in zip(texts, boxes, scores):
            x_coords = [point[0] for point in box]
            y_coords = [point[1] for point in box]
            bbox = [min(x_coords), min(y_coords), max(x_coords), 
                    max(y_coords)]
            
            item = {
                'text': text,
                'bbox': bbox,
            }
            if score is not None:
                item['confidence'] = score
                
            extracted_items.append(item)
        
        return extracted_items
    
    except Exception as e:
        return [{"error": f"Error reading image: {e}"}]

# 1. Define the list of tools
tools = [paddle_ocr_read_document]

# 2. Set up the OpenAI GPT model
llm = ChatOpenAI(
    model="gpt-5-mini", 
    temperature=1 
)

# 3. Create the OpenAI-compatible prompt
prompt = ChatPromptTemplate.from_messages([
    (
        "system",
        "You are a helpful assistant designed to extract information "+
        "from documents. "
            "You have access to this tool: "
            "Paddle OCR tool to extract raw texts, bounding boxes for "+
            "each text and confidence score from images "
        ),
        ("user", "{input}"),
        MessagesPlaceholder(variable_name="agent_scratchpad"),
    ]
)

# 4. Create a proper tool-calling agent
agent = create_tool_calling_agent(llm, tools, prompt)

# 5. Set up the AgentExecutor to run the tool-enabled loop
agent_executor = AgentExecutor(agent=agent, tools=tools, verbose=True)

task = """
Process the document at 'receipt.jpg' and evaluate that 
the total is correct.
"""
response = agent_executor.invoke({"input": task})

```
