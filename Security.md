# Security Policy

## Supported Versions

| Version | Supported |
|---|---|
| v4.x (current) | ✅ |
| v3.x | ⚠️ Critical fixes only |
| v2.x and below | ❌ |

---

## Reporting a Vulnerability

If you discover a security vulnerability, please **do not open a public GitHub issue**.

Instead, report it by:
1. Opening a **private security advisory** via GitHub Security tab
2. Or emailing the maintainer directly (see GitHub profile)

Include:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

You will receive a response within 72 hours.

---

## Security Model

MedInsight AI is designed with a local-first, privacy-preserving architecture:

### What runs locally
- All LLM inference (via Ollama)
- All document parsing (PyMuPDF, python-docx, Tesseract)
- All PDF generation (ReportLab)
- Session state (Streamlit in-memory)

### What is never transmitted
- Patient data
- Document contents
- Analysis results
- User inputs of any kind

### Network requests made by the app
| Destination | Purpose | Can be blocked? |
|---|---|---|
| `localhost:11434` | Ollama LLM inference | No (required) |
| `fonts.googleapis.com` | Inter / Merriweather fonts (CSS) | Yes — app functions without them |

### No authentication by default
The app runs on `localhost:8501` with no authentication. If deploying on a shared network or server:
- Place behind a reverse proxy with HTTP Basic Auth (nginx, Caddy)
- Use Streamlit's built-in `[server]` config to bind to `127.0.0.1` only
- Do not expose port 8501 publicly

---

## Patient Data Handling

- No patient data is stored to disk by default
- All analysis results live in `st.session_state` (cleared on browser refresh)
- No database, no log files containing patient data
- Downloaded PDFs/JSONs are the user's responsibility to handle per local HIPAA/GDPR requirements

---

## Dependencies

Security patches in dependencies are applied promptly. Run:
```bash
pip install --upgrade -r requirements.txt
```
to get the latest patched versions.
