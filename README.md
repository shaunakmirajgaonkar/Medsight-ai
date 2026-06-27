# MedInsight AI 🏥

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![LLM](https://img.shields.io/badge/LLM-Ollama_Local-green?style=flat-square)
![Frontend](https://img.shields.io/badge/Frontend-Streamlit-red?style=flat-square)
![Privacy](https://img.shields.io/badge/Privacy-100%25_Local-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)
![Status](https://img.shields.io/badge/Status-Active-success?style=flat-square)

> 100% local AI-powered clinical decision support platform — no cloud, no API keys, no patient data leaves your device.

Upload patient medical records, lab reports, and prescriptions. Get differential diagnoses, risk predictions, drug interaction checks, and evidence-backed clinical recommendations — all running locally on your machine via Ollama.

---

## What is MedInsight AI?

MedInsight AI is a **clinical decision support system (CDSS)** built for licensed physicians. It processes medical documents (PDF, DOCX, images) and structured patient data through a local large language model to produce structured clinical analysis — differential diagnoses ranked by confidence, lab value interpretation, 10-year cardiovascular risk predictions, drug interaction checks, and prioritised recommendations citing AHA/ACC, ADA, NICE, and ESC guidelines.

> ⚠️ **For physician use only.** This is a decision *support* tool. All outputs must be validated by a licensed clinician. It does not replace clinical judgment, examination, or diagnosis.

---

## Demo

```
Upload PDF/DOCX → Auto-extract patient data → Run Local Analysis → Download PDF Report
```

**Example cases included:**
```
Case 1: Diabetic male, 58yr — exertional chest pain, elevated Troponin I, ST depression
Case 2: Postpartum female, 32yr — hypothyroidism + iron deficiency anaemia
Case 3: Hypertensive male, 74yr — atrial fibrillation + cognitive decline on Warfarin
```

---

## Features

| Feature | Description |
|---|---|
| 🔒 **100% Local** | All inference via Ollama — patient data never leaves your machine |
| 📎 **Document Upload** | Auto-extracts text from PDF, DOCX, TXT, and scanned images (OCR) |
| 🧬 **Differential Diagnosis** | Ranked conditions with ICD-10 codes, confidence %, and clinical reasoning |
| 🔬 **Lab Interpretation** | Every value flagged CRITICAL / HIGH / LOW / NORMAL with clinical context |
| 📊 **Risk Prediction** | Cardiovascular, diabetic, and complication risk % with evidence basis |
| 💊 **Drug Interactions** | Major / Moderate / Minor interactions with mechanism and monitoring advice |
| 📋 **Recommendations** | URGENT → ROUTINE actions citing AHA/ACC, ADA, NICE, ESC guidelines (Grade A/B/C) |
| 📄 **PDF Report** | Professional clinical report export via ReportLab |
| 🔄 **JSON Repair** | Auto-fixes malformed LLM output (trailing commas, truncation, smart quotes) |
| 📂 **Session History** | All analyses preserved in-session with reload and re-export |

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Streamlit |
| **LLM Runtime** | Ollama (local) |
| **Recommended Models** | `llama3.1:8b`, `mistral:7b`, `llama3.2:3b` |
| **Document Parsing** | PyMuPDF (PDF), python-docx (DOCX), Tesseract OCR (images) |
| **PDF Reports** | ReportLab |
| **Language** | Python 3.10+ |

---

## Quickstart

### 1. Clone the repo
```bash
git clone https://github.com/shaunakmirajgaonkar/medinsight-ai.git
cd medinsight-ai
```

### 2. Install dependencies
```bash
pip install -r requirements.txt

# Optional: OCR support for scanned documents
brew install tesseract          # macOS
sudo apt install tesseract-ocr  # Ubuntu/Debian
```

### 3. Install Ollama and pull a model
```bash
# Install Ollama
brew install ollama              # macOS
curl -fsSL https://ollama.com/install.sh | sh  # Linux

# Pull a model (choose based on your hardware)
ollama pull llama3.1:8b         # Best quality — needs 8 GB RAM
ollama pull llama3.2:3b         # Fast — works on 4 GB RAM
ollama pull mistral:7b          # Alternative
```

### 4. Start Ollama
```bash
ollama serve
```

### 5. Run the app
```bash
streamlit run Medsight_ai.py
```

Open **http://localhost:8501** in your browser.

---

## Hardware Requirements

| Model | RAM Required | Speed (M-series Mac) |
|---|---|---|
| `llama3.2:3b` | 4 GB | ~15–30 seconds |
| `llama3.1:8b` | 8 GB | ~30–90 seconds |
| `mistral:7b` | 8 GB | ~30–90 seconds |
| `mistral-nemo:12b` | 16 GB | ~60–120 seconds |

GPU (NVIDIA / AMD / Apple Silicon) significantly speeds up inference. Apple M-series gets automatic Metal acceleration via Ollama.

---

## How It Works

```
User uploads PDF/DOCX
        ↓
Text extracted (PyMuPDF / python-docx / Tesseract OCR)
        ↓
Section-aware parser maps content → 8 structured patient fields
        ↓
Structured prompt sent to local Ollama LLM
        ↓
JSON response repaired (trailing commas, truncation, encoding issues)
        ↓
Structured analysis rendered across 5 clinical modules
        ↓
PDF report generated via ReportLab
```

---

## Project Structure

```
medinsight-ai/
├── Medsight_ai.py          ← Main Streamlit application (single file)
├── requirements.txt        ← Python dependencies
├── .env.example            ← Environment variable template
├── .gitignore
├── LICENSE                 ← MIT License
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── ACKNOWLEDGMENTS.md
└── docs/
    ├── architecture.md     ← System design overview
    ├── clinical_modules.md ← Details on each analysis module
    └── supported_formats.md← Document format support matrix
```

---

## Clinical Modules

### 🧬 Differential Diagnosis
Returns ranked differential diagnoses with:
- Probability (High / Moderate / Low)
- Confidence percentage (0–100%)
- Clinical reasoning paragraph
- ICD-10 code
- Key supporting features
- Arguments against the diagnosis

### 🔬 Lab Interpretation
Processes every lab value and flags:
- **CRITICAL** — immediate clinical action required
- **HIGH / LOW** — out of reference range
- **NORMAL** — within reference range
- Clinical significance and recommended action

### 📊 Risk Prediction
Calculates disease/complication risk based on:
- Framingham Risk Score (cardiovascular)
- UKPDS Risk Engine (diabetic complications)
- ADA 2024 risk stratification
- Splits into modifiable vs non-modifiable risks

### 💊 Drug Interactions
Checks all listed medications for:
- **Major** — contraindicated or life-threatening
- **Moderate** — requires monitoring or dose adjustment
- **Minor** — generally manageable
- Pharmacokinetic / pharmacodynamic mechanism
- Recommended clinical action and monitoring parameters

### 📋 Recommendations
Evidence-graded clinical actions:
- **Grade A** — strong RCT evidence
- **Grade B** — observational / cohort evidence
- **Grade C** — expert consensus / guideline opinion
- Priority: URGENT → HIGH → MODERATE → ROUTINE
- Guideline citations: AHA/ACC 2023, ADA Standards 2024, NICE UK, ESC 2023

---

## Supported Document Formats

| Format | Library | Notes |
|---|---|---|
| `.pdf` | PyMuPDF (fitz) | Text layer extraction |
| `.docx` | python-docx | Paragraphs + table cells |
| `.txt`, `.md` | Built-in | UTF-8 / Latin-1 |
| `.png`, `.jpg`, `.jpeg` | Tesseract OCR | Requires `tesseract` installed |
| `.bmp`, `.tiff`, `.webp` | Tesseract OCR | Requires `tesseract` installed |

---

## Privacy

- ✅ **Zero data transmission** — all processing is local
- ✅ **No API keys** — no accounts or subscriptions required
- ✅ **Air-gapped capable** — works fully offline after model download
- ✅ **No logging to external services** — patient data stays on your machine
- ✅ **Open source** — full source code auditable

---

## Limitations

- Local models (3B–8B parameters) may produce hallucinated values or miss rare diagnoses — always verify
- JSON parsing errors may occur with very small models (`llama3.2:3b`) on complex cases — use `llama3.1:8b` for best results
- OCR accuracy depends on scan quality
- Not validated in clinical trials
- Not a substitute for clinical judgment, physical examination, or specialist opinion

---

## Contributing

Contributions are welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting a pull request.

```bash
# Fork → Clone → Create branch → Make changes → Test → PR
git checkout -b feature/your-feature-name
```

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Acknowledgments

See [ACKNOWLEDGMENTS.md](ACKNOWLEDGMENTS.md) for libraries and resources used.

---

## Author

**Shaunak Mirajgaonkar**  
BE Computer Engineering, MMCOE Pune (SPPU)  
[GitHub](https://github.com/shaunakmirajgaonkar) · [LinkedIn](https://linkedin.com/in/shaunakmirajgaonkar)

---

<p align="center">
Built with ❤️ for physicians who value patient privacy · 100% local · zero cloud dependency
</p>
