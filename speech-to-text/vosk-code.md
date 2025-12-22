# 🟢 VOSK - Code

* <mark style="color:purple;background-color:purple;">**Used io.BytesIO for inmemory**</mark>
* <mark style="color:purple;background-color:purple;">**We get video in memory using requests**</mark>
* <mark style="color:purple;background-color:purple;">**We convert Video to Audio using ffmpeg ⇒ wav format 16kHz, mono channel**</mark>
* <mark style="color:purple;background-color:purple;">**We will pass this audio to KaldiRecognizer in chunks of 4000 frames (4000/ 16kHz = 0.25s)**</mark>
* <mark style="color:purple;background-color:purple;">**We did this in loop as our video was of 2mins max**</mark>
* <mark style="color:purple;background-color:purple;">**Not did any preprocessing as I was not aware of it also since it was interviews people used to give it in a silent background**</mark>
* <mark style="color:$danger;background-color:purple;">**Transcription time for 25-sec audio**</mark><mark style="color:$danger;background-color:purple;">: \~2 to 4 seconds</mark>

<mark style="color:purple;background-color:purple;">**Post Processing:**</mark>

* <mark style="color:purple;background-color:purple;">**Zipf frequency = how common a word is in real English**</mark>
  * <mark style="color:purple;background-color:purple;">**Higher number → word is very common**</mark>
  * <mark style="color:purple;background-color:purple;">**Lower number → word is rare or possibly invalid (Cut off value is 2)**</mark>
  * <mark style="color:purple;background-color:purple;">**If your domain words are missing in the corpus, they may get low Zipf scores**</mark>
  * <mark style="color:purple;background-color:purple;">**Replace using dictionary if direct match found**</mark>
  * <mark style="color:purple;background-color:purple;">**Replace it using nearest match from your dictionary using Levenshtein**</mark>

```python
import requests
import io
import json
import ffmpeg
from vosk import Model, KaldiRecognizer

VIDEO_URL = "INTRANET_VIDEO_URL"
HEADERS = {"Authorization": "Bearer YOUR_TOKEN"}  # if needed

# 1. Stream video/audio from intranet
response = requests.get(VIDEO_URL, headers=HEADERS, stream=True)
raw_bytes = io.BytesIO(response.content)

# 2. Convert video/audio to WAV (16kHz, mono) in-memory
out, _ = (
    ffmpeg
    .input('pipe:0')
    .output('pipe:1', format='wav', ac=1, ar=16000)
    .run(input=raw_bytes.read(), capture_stdout=True, capture_stderr=True)
)

audio_stream = io.BytesIO(out)

# 3. Feed into Vosk
model = Model("vosk-model-en-in-0.5")
rec = KaldiRecognizer(model, 16000)

final_text = ""

while True:
    chunk = audio_stream.read(4000)
    if not chunk:
        break

    if rec.AcceptWaveform(chunk):
        result = json.loads(rec.Result())
        final_text += result.get("text", "") + " "

final_text += json.loads(rec.FinalResult()).get("text", "")

print("Transcript:", final_text)

```
