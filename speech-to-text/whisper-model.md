# Whisper - Model

* Upgraded to whisper model in 2023
* Open-sourced by OpenAI
* Models released: `tiny, base, small, medium, large`
* Trained on 680,000+ hours of multilingual data
* Included strong performance on Indian English + accented speech
*

    <figure><img src="../.gitbook/assets/{1DA0819D-A479-482C-A28C-19AF78304246}.png" alt=""><figcaption></figcaption></figure>

**Architecture:**

* Transformer-based encoder–decoder model

**Input to Transformer:**

* Raw audio (waveform) is converted into a log-Mel spectrogram
* Spectrogram is split into small time windows
* Each window becomes a feature vector
* These vectors are like “tokens” for the Transformer

