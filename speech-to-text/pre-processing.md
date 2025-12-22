# 🟢 Pre Processing

**Noise Removal:**

* <mark style="color:purple;background-color:purple;">**I used the first short segment of audio as a noise profile since most interview recordings begin with silence, making it a reliable and lightweight way to estimate background noise automatically.**</mark>
* <mark style="color:purple;background-color:purple;">**Takes the first 0.5 seconds of the audio**</mark>
* <mark style="color:purple;background-color:purple;">**Assumes this part mostly contains background noise / silence**</mark>
* <mark style="color:purple;background-color:purple;">**Calculates the average loudness of the noise**</mark>
* <mark style="color:purple;background-color:purple;">**Higher value → more background noise**</mark>
* <mark style="color:purple;background-color:purple;">**Calculates the average loudness of the full audio**</mark>
* <mark style="color:purple;background-color:purple;">**Represents the main speech signal**</mark>
* <mark style="color:purple;background-color:purple;">**Compares noise vs actual speech**</mark>
* <mark style="color:purple;background-color:purple;">**If noise is more than 30% of the signal → considered noisy**</mark>
* <mark style="color:purple;background-color:purple;">**If noisy → apply light noise reduction**</mark>
* <mark style="color:purple;background-color:purple;">**If no noise then don't do any noise reduction**</mark>
* <mark style="color:purple;background-color:purple;">**For noise calculation, we took audio sample using librosa and then took mean of the frequencies**</mark>
* <mark style="color:purple;background-color:purple;">**If noise found then using noisereduce library we reduced it**</mark>

```python
import numpy as np
import librosa
import noisereduce as nr

audio, sr = librosa.load("input.wav", sr=16000)

# Estimate noise from first 0.5 seconds
noise_sample = audio[: int(0.5 * sr)]

noise_level = np.mean(np.abs(noise_sample))
signal_level = np.mean(np.abs(audio))

# If noise is more than 30% of signal → reduce it
if noise_level / signal_level > 0.3:
    clean_audio = nr.reduce_noise(y=audio, sr=sr)
else:
    clean_audio = audio

```
