# MedInsight AI — Architecture

## Overview

MedInsight AI is a single-file Streamlit application (`Medsight_ai.py`) with no backend server, no database, and no external API calls. All intelligence runs locally via Ollama.

```
┌─────────────────────────────────────────────────────────┐
│                    Browser (localhost:8501)             │
│                    Streamlit UI                         │
└─────────────────┬───────────────────────────────────────┘
                  │ st.session_state
┌─────────────────▼───────────────────────────────────────┐
│                  Medsight_ai.py                         │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐   │
│  │  Document    │  │  Patient     │  │  Results     │   │
│  │  Extractor   │  │  Form        │  │  Renderer    │   │
│  └──────┬───────┘  └──────┬───────┘  └──────▲───────┘   │
│         │                 │                 │           │
│         └────────┬────────┘                 │           │
│                  │ patient dict             │           │
│         ┌────────▼────────┐                 │           │
│         │  Prompt Builder │                 │           │
│         └────────┬────────┘                 │           │
│                  │ HTTP POST                │           │
└──────────────────┼──────────────────────────┘           │
                   │                            │
┌──────────────────▼────────────────────────────┐
│              Ollama (localhost:11434)          │
│              Local LLM Inference               │
│              llama3.1:8b / mistral:7b          │
└───────────────────────────────────────────────┘
                   │ raw JSON string
         ┌─────────▼──────────┐
         │    fix_json()      │ — repair malformed output
         └─────────┬──────────┘
                   │ parsed dict
         ┌─────────▼──────────┐
         │  Results Renderer  │ — tabs, cards, badges
         └─────────┬──────────┘
                   │
         ┌─────────▼──────────┐
         │  ReportLab PDF     │ — download button
         └────────────────────┘
```

## Data Flow

### Document Upload Path
```
File upload → extract_file() → raw text
     → smart_parse() → {demographics, vitals, labs, ...}
     → pre-fill Streamlit form fields
```

### Analysis Path
```
Patient form fields → build_prompt() → Ollama HTTP POST
     → raw LLM response → fix_json() → dict
     → render_results() → UI tabs
     → generate_pdf_report() → bytes → download button
```

## Key Components

### `extract_file()`
Dispatches to the correct extractor by file extension:
- PDF → PyMuPDF `fitz.open()` — reads text layer
- DOCX → python-docx — walks element body (paragraphs + table cells)
- TXT/MD → raw decode (UTF-8 / Latin-1 fallback)
- Images → pytesseract OCR

### `smart_parse()`
Section-aware parser. Two passes:
1. Detect section headers (regex against `SECTION_MAP`)
2. Assign content lines to the matching field bucket

Handles:
- Fused demographic lines (`Full Name: JohnAge: 52`)
- Table header row filtering
- Status marker stripping from vitals (`⚠ HIGH`, `✓ NORMAL`)
- Medication section-header leakage

### `fix_json()`
10-step LLM output repair:
1. Strip markdown fences
2. Extract outermost `{...}`
3. Replace smart/curly quotes
4. Remove JS-style `//` comments
5. Fix trailing commas before `}` or `]`
6. Fix missing commas between array strings
7. Escape bare newlines/tabs inside string values
8. Wrap output if missing opening `{`
9. Close truncated JSON (count `{[` vs `}]`)
10. Final trailing-comma pass

### `generate_pdf_report()`
ReportLab Platypus document builder.
Sections: Header → Summary → Urgent Flags → Differentials → Labs → Risk → Recommendations → Drug Interactions → Follow-up → Patient Data Reference → Disclaimer

## Session State Keys

| Key | Type | Description |
|---|---|---|
| `doc_text` | `str` | Raw combined extracted text |
| `doc_log` | `list[tuple]` | (filename, method, char_count) per file |
| `doc_fields` | `dict` | Parsed field values from document |
| `doc_ready` | `bool` | Whether a document has been processed |
| `last_result` | `dict` | Most recent analysis result |
| `last_elapsed` | `float` | Inference time in seconds |
| `last_model` | `str` | Model used for last analysis |
| `last_tps` | `float` | Tokens per second for last analysis |
| `last_patient` | `dict` | Patient data used for last analysis |
| `history_log` | `list[dict]` | All analyses this session |
