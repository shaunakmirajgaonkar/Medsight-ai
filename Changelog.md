# Changelog

All notable changes to MedInsight AI are documented here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [v4.0.0] — 2026-06-27

### Added
- Complete UI redesign — clean white + emerald green medical theme
- Document upload gate: PDF, DOCX, TXT, and image (OCR) support
- Section-aware document parser — maps extracted text into 8 structured patient fields
- Smart field splitter for fused demographic lines (e.g. `Full Name: JohnAge: 52`)
- Table cell extraction for DOCX files (python-docx paragraphs-only was missing all table data)
- Professional PDF report export via ReportLab — multi-section clinical report
- JSON auto-repair (`fix_json`) — handles trailing commas, truncated output, smart quotes, missing array commas, JS comments
- Automatic retry with simplified prompt on JSON parse failure
- Confidence percentage bar per differential diagnosis
- Evidence grade (A/B/C) on all recommendations
- Modifiable vs non-modifiable split on risk predictions
- Drug interaction mechanism (PK/PD) and monitoring parameters
- Follow-up red flags section
- Session history with per-session PDF and JSON export

### Fixed
- Vitals field incorrectly populated with clinical notes text
- Medications field leaking section header lines (e.g. "8. PHYSICIAN CLINICAL IMPRESSION")
- Demographics displayed as single fused line
- Table header rows (Parameter | Value | Status) appearing in vitals/labs fields
- Status markers (✓ NORMAL, ⚠ HIGH) included in vitals field values

### Changed
- DOCX extractor now walks full document body in reading order (paragraphs + tables)
- Parser uses section-header detection instead of keyword matching on individual lines
- JSON parser now attempts 3 repair strategies before surfacing error to user

---

## [v3.0.0] — 2026-06-26

### Added
- Advanced analysis schema with `key_features`, `against`, `clinical_impression`
- Risk prediction modifiability flag
- Drug interaction mechanism field
- Follow-up red flags

### Fixed
- Smart parser false-positive matches on words like "probable" (containing "BP")
- `HR` matching in "high-risk" context

---

## [v2.0.0] — 2026-06-25

### Added
- Document upload tab with multi-file support
- Basic field extraction from plain text and PDF
- Session history tab

### Changed
- UI rebuilt from dark navy theme to white/green clinical theme

---

## [v1.0.0] — 2026-06-24

### Added
- Initial release
- Ollama local inference integration
- 6 clinical analysis modules (differentials, labs, risk, drugs, recs, follow-up)
- 3 built-in demo patient cases
- Sidebar model selector and module toggles
