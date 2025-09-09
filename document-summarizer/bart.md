# 🔴 BART

* **Architecture:** Encoder–Decoder (like a seq2seq transformer).
* **Training objective:** Denoising autoencoder.
  * Input: Corrupted sentence.
  * Output: Original sentence.
* **For summarization:**
  * Encoder → encodes input document.
  * Decoder → generates summary.
* **Strength:** Very good for long-form abstractive summarization.
