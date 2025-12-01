# Pre Processing

**Noise Removal:**

* I used the first short segment of audio as a noise profile since most interview recordings begin with silence, making it a reliable and lightweight way to estimate background noise automatically.
* Takes the first 0.5 seconds of the audio
* Assumes this part mostly contains background noise / silence
* Calculates the average loudness of the noise
* Higher value → more background noise
* Calculates the average loudness of the full audio
* Represents the main speech signal
* Compares noise vs actual speech
* If noise is more than 30% of the signal → considered noisy
* If noisy → apply light noise reduction
* If no noise then don't do any noise reduction

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
