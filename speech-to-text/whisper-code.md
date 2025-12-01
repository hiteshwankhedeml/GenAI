# Whisper - Code

* Whisper internally breaks audio into 30-second chunks
* Then processes each chunk one by one
* Finally merges them into a single transcript
* We had used 32GB RAM CPU but it used to take a long time
* We moved Whisper inference to an NVIDIA T4 GPU, which reduced transcription time for a 2-minute response from several minutes on CPU to roughly 20–40 seconds
*

    | Hardware (2023)    | Approx response time | Comments              |
    | ------------------ | -------------------- | --------------------- |
    | CPU (32 GB RAM)    | **4 – 8 minutes**    | Very slow, CPU-bound  |
    | NVIDIA T4 (16 GB)  | **20 – 40 seconds**  | Most common cloud GPU |
    | RTX 3060 (12 GB)   | **10 – 20 seconds**  | Better performance    |
    | A10 / A10G (24 GB) | **8 – 15 seconds**   | Strong inference GPU  |

```python
import io
import requests
from transformers import pipeline

# 1. Load Whisper via Hugging Face
asr = pipeline(
    task="automatic-speech-recognition",
    model="openai/whisper-small"   # Phase-2 choice (2022)
)

# 2. Video URL (intranet or external)
VIDEO_URL = "YOUR_VIDEO_URL_HERE"

# If intranet requires auth, use headers
HEADERS = {
    # "Authorization": "Bearer YOUR_TOKEN",
    # "Cookie": "YOUR_SESSION_COOKIE"
}

# 3. Stream video/audio into memory (no file saved)
response = requests.get(VIDEO_URL, headers=HEADERS)
video_bytes = response.content

# 4. Convert to in-memory file
audio_stream = io.BytesIO(video_bytes)

# 5. Send to Whisper
result = asr(audio_stream)

print("\nTranscript:\n")
print(result["text"])
```
