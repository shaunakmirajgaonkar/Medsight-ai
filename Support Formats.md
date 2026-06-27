# Supported Document Formats

MedInsight AI can extract text from the following file types:

---

## PDF (`.pdf`)

**Library:** PyMuPDF (`fitz`)  
**Method:** Text layer extraction  
**Requirements:** `pip install pymupdf`

Works best with:
- Digitally generated PDFs (hospital system exports, lab reports)
- Scanned PDFs with embedded text layer (searchable PDFs)

Does not work with:
- Purely scanned PDFs with no text layer → use image OCR instead

---

## Word Document (`.docx`)

**Library:** python-docx  
**Method:** Paragraphs + table cells (full document body traversal)  
**Requirements:** `pip install python-docx`

The extractor walks the full document body in reading order, extracting:
- Paragraph text
- Table cells (2-column → `Label: Value`, 3+ column → pipe-separated)

Works best with:
- Clinical notes, referral letters, discharge summaries
- Lab result tables with Parameter / Value / Reference / Status columns

---

## Plain Text (`.txt`, `.md`, `.csv`, `.log`)

**Library:** Python built-in  
**Method:** UTF-8 decode with Latin-1 fallback  
**Requirements:** None

Works best with:
- HL7 export files
- CSV lab results
- Copy-pasted clinical notes

---

## Images (`.png`, `.jpg`, `.jpeg`, `.bmp`, `.tiff`, `.webp`)

**Library:** pytesseract + Pillow  
**Method:** Tesseract OCR engine  
**Requirements:**
```bash
pip install pytesseract Pillow
brew install tesseract          # macOS
sudo apt install tesseract-ocr  # Ubuntu/Debian
```

Works best with:
- High-resolution scanned documents (300 DPI+)
- Black-and-white or greyscale scans
- Documents with clear font and good contrast

OCR accuracy tips:
- Scan at 300 DPI or higher
- Ensure good lighting and minimal shadows
- Avoid heavily compressed JPEGs

---

## Parser Output Fields

Regardless of input format, extracted text is mapped to 8 fields:

| Field | Typical source |
|---|---|
| `demographics` | Patient header, demographics table |
| `chief_complaint` | Chief Complaint / Presenting History section |
| `history` | Past Medical / Surgical / Family History section |
| `vitals` | Vital Signs table |
| `symptoms` | Symptoms / Review of Systems section |
| `labs` | Laboratory Investigations / Results tables |
| `medications` | Current Medications table |
| `notes` | Physician Impression / Assessment and Plan |

---

## Multi-file Upload

Multiple files can be uploaded simultaneously. Text is extracted from each file and concatenated with `=== filename ===` separators before parsing. Useful for:
- Uploading lab report PDF + clinical notes DOCX together
- Combining multiple pages scanned as separate images
