---
hidden: true
---

# ✈️ Code - Document Understanding with ADE

* To parse documents and extract key-value pairs using a single API
* &#x20;

```python
# General imports

import os
import json
import pymupdf
import pandas as pd
from pathlib import Path
from dotenv import load_dotenv
from IPython.display import display, IFrame, Markdown, HTML
from IPython.display import Image as DisplayImage
from PIL import Image as PILImage, ImageDraw

# Imports specific to Agentic Document Extraction

from landingai_ade import LandingAIADE
from landingai_ade.types import ParseResponse, ExtractResponse


```
