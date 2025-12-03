# 2 way - Video Analysis

* **Face Detection** to ensure exactly one person is present in the video.
* **Liveness Detection** to confirm the candidate is a real human and not a photo/video replay.
* **Lip-Sync Check** to verify mouth movements match the Whisper STT audio.
* **Spoofing Detection** to prevent fraud using deepfakes or pre-recorded clips.
* **Face Consistency Check** to ensure the same person stays throughout the interview.
* **Frame Drop / Tampering Check** to detect any edited or manipulated video segments.
* **Background Noise Classification** to tag high-noise environments.
* **Illumination Check** to ensure face is visible and usable.
* **Multiple Face Detection** to reject cases where someone else appears or helps.
* **Silence vs Speaking Detection** to ensure candidate is speaking when STT sends empty text.
