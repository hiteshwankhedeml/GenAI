# Evolution of OCR

**Tesseract:**

* stands for the traditional, procedural computer vision approach ⇒ Lots of rules, lots of steps
* Supports 100+ languages
* Works well for clean, high resolution printed documents
* Lightweight CPU
* Cannot work on handwritten data or complex patterns



**PaddleOCR:**

* PaddleOCR is representative of Deep learning era ⇒ Using Neural network
* 2 step pipeline using deep learning
  * **Text detection model:** find regions that contain text
  * **Text recognition model:** Read the content of each text region
*
