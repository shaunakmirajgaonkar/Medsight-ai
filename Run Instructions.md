MedInsight AI — Run Instructions
=================================

PREREQUISITES
-------------
- Python 3.10 or higher
- Ollama installed (https://ollama.com)
- At least 4 GB free RAM (8 GB recommended)


STEP 1 — Clone the repository
------------------------------
  git clone https://github.com/shaunakmirajgaonkar/medinsight-ai.git
  cd medinsight-ai


STEP 2 — Install Python dependencies
--------------------------------------
  pip install -r requirements.txt

  Optional (for OCR on scanned documents):
    macOS:          brew install tesseract
    Ubuntu/Debian:  sudo apt install tesseract-ocr
    Windows:        Download from https://github.com/UB-Mannheim/tesseract/wiki


STEP 3 — Install Ollama
------------------------
  macOS:   brew install ollama
  Linux:   curl -fsSL https://ollama.com/install.sh | sh
  Windows: Download from https://ollama.com/download/windows


STEP 4 — Pull a model
----------------------
  Fast (4 GB RAM, ~15-30s per analysis):
    ollama pull llama3.2:3b

  Best quality (8 GB RAM, ~30-90s per analysis):
    ollama pull llama3.1:8b

  Alternative:
    ollama pull mistral:7b


STEP 5 — Start Ollama
----------------------
  ollama serve

  Leave this terminal open. Ollama runs on http://localhost:11434


STEP 6 — Launch MedInsight AI
-------------------------------
  Open a new terminal in the project folder, then:

    streamlit run Medsight_ai.py

  Open your browser at: http://localhost:8501


USAGE
------
1. Go to the "Upload Document" tab
2. Drop a PDF, DOCX, or image of a medical record
3. Fields are auto-filled in the "Patient Data" tab
4. Review and edit the extracted data
5. Click "Run Clinical Analysis"
6. View results in the "Analysis" tab
7. Download the PDF report


DEMO CASES
----------
No document needed — load a pre-filled demo from the sidebar:
  • Case 1: Diabetic male, 58yr — chest pain, elevated Troponin
  • Case 2: Postpartum female, 32yr — hypothyroidism + iron deficiency
  • Case 3: Hypertensive male, 74yr — AF + cognitive decline on Warfarin


STOPPING THE APP
-----------------
  Ctrl+C in the terminal running Streamlit


DISCLAIMER
----------
MedInsight AI is a clinical decision SUPPORT tool for licensed physicians only.
All outputs must be validated by a licensed clinician. Not a substitute for
clinical judgment, examination, or diagnosis.
