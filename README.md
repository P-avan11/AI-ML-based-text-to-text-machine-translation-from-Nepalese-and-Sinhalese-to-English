# Multilingual OCR & Machine Translation System

A full-stack, intelligent document processing and translation web application that converts uploaded images or multi-page PDFs containing native South Asian scripts (**Nepali** and **Sinhala**) into fluent, contextually accurate **English**.

The system features a **hybrid machine translation architecture** that defaults to cloud endpoints for maximum speed but seamlessly hot-swaps to a localized deep learning transformer model (**Facebook's NLLB-200**) if the host environment drops offline.

---

## 🚀 Key Features
* **Multi-Format Ingestion:** Seamlessly processes digital screenshots, image uploads (PNG/JPG), and multi-page PDF documents.
* **Script-Optimized Computer Vision:** Custom OpenCV pipeline custom-tuned to preserve thin character matras and structural strokes in Devanagari and Sinhala scripts.
* **Dual-PSM Dynamic Voting:** OCR engine concurrently evaluates text layouts using dynamic Page Segmentation Modes (PSM 3 and PSM 6) to eliminate character drop-outs.
* **Hybrid & Offline Translation:** Robust networking fallback layer that hot-swaps execution to a localized 600M-parameter NLLB transformer model during network disruptions.
* **Secure Session Management:** Built-in registration and login authentication infrastructure backed by SQLite3.

---

## 🛠️ Tech Stack & Dependencies
* **Core Backend:** Python, Flask
* **Document Processing & Vision:** OpenCV (cv2), Tesseract OCR, pdf2image
* **Deep Learning Frameworks:** PyTorch, Hugging Face Transformers (NLLB-200-Distilled-600M)
* **Database Layer:** SQLite3

---

## 📂 Project Architecture
```text
├── app.py                  # Main Flask application, routing, and file pipelines
├── database.py             # SQLite configuration and user auth interfaces
├── ocr_module.py           # Script-optimized Tesseract configurations and PSM filters
├── preprocess.py           # Low-level OpenCV pixel upscaling and image cleaning filters
├── translation_module.py   # Hybrid cloud/offline translation engine & text chunkers
├── download_model.py       # Helper dependency script to pull model weights locally
└── requirements.txt        # Managed Python library dependencies




Installation & Setup
1. Prerequisites
Ensure you have Tesseract OCR and Poppler installed on your operating system and configured in your environment paths.

Tesseract OCR Path: C:\Program Files\Tesseract-OCR\tesseract.exe

Poppler Binaries: Required by pdf2image to convert PDF layouts into processable image matrices.

2. Environment Setup
Clone the repository and install the required Python packages:

Bash
# Clone the repository
git clone [https://github.com/yourusername/multilingual-ocr-translation.git](https://github.com/yourusername/multilingual-ocr-translation.git)
cd multilingual-ocr-translation

# Install required Python dependencies
pip install -r requirements.txt
3. Initialize Databases & Download ML Models
Run the setup modules to prepare your localized SQLite schema and download the distilled NLLB-200 transformer weights from Hugging Face:

Bash
# Initialize the SQLite database schema
python database.py

# Download the deep learning tokenizer and model weights
python download_model.py
4. Launch the Application
Start the localized development server:

Bash
python app.py
Open your preferred web browser and navigate to .

🧠 How the Pipeline Works
Ingestion & Parsing: Uploaded documents are converted to high-resolution images via pdf2image and sent to the data processing pipeline.

Vision Enhancement: Images are upscaled to a minimum threshold of 3000px wide using Lanczos4 interpolation, and localized contrast is maximized using CLAHE.

Character Extraction: Tesseract processes the enhanced matrices under specific native script libraries (nep+hin / sin).

Contextual Translation: The translation wrapper evaluates network connection states. If online, it chunks data to safe text sizes and pushes it to cloud networks. If offline, the PyTorch engine initializes, parses pre-translation synonym maps, and translates using the localized model.
