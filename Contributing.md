# Contributing to MedInsight AI

Thank you for considering contributing to MedInsight AI. Contributions of all kinds are welcome — bug fixes, documentation improvements, new clinical modules, better document parsers, UI enhancements, and test coverage.

---

## Getting Started

### 1. Fork and clone
```bash
git clone https://github.com/YOUR_USERNAME/medinsight-ai.git
cd medinsight-ai
```

### 2. Create a branch
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/bug-description
```

### 3. Set up environment
```bash
pip install -r requirements.txt
brew install tesseract   # macOS, for OCR tests
ollama pull llama3.2:3b  # smallest model for local testing
ollama serve
```

### 4. Make changes and test
```bash
streamlit run Medsight_ai.py
```

### 5. Commit and push
```bash
git add .
git commit -m "feat: add X" # or "fix: correct Y"
git push origin feature/your-feature-name
```

### 6. Open a Pull Request
Go to the original repository and open a PR with a clear description of what changed and why.

---

## Contribution Areas

### High Priority
- [ ] Better OCR pre-processing (deskew, contrast, denoise)
- [ ] Support for `.xlsx` lab result files
- [ ] Support for HL7 FHIR JSON input
- [ ] Faster JSON schema enforcement (grammar-constrained generation)
- [ ] Model benchmarks across llama3.1:8b / mistral / medllama2

### Medium Priority
- [ ] Dark mode toggle
- [ ] Multi-patient comparison view
- [ ] Saved patient profiles (local SQLite)
- [ ] ICD-10 code autocomplete in manual entry

### Documentation
- [ ] Video walkthrough / demo GIF
- [ ] Clinical validation methodology notes
- [ ] Per-module technical documentation

---

## Code Style

- Follow PEP 8 for Python code
- Keep functions short and single-purpose
- Add a docstring to every new function
- Use type hints where practical
- Test document extraction changes against the provided `demo/patient_medical_record_demo.docx`

---

## Reporting Bugs

Open an issue with:
- OS and Python version
- Model used (`ollama list`)
- Steps to reproduce
- Error message or screenshot
- Expected vs actual behaviour

---

## Clinical Content

If you are a clinician and notice a clinical error in the system prompt, differential logic, drug interaction data, or guideline citations — please open an issue labelled `clinical-accuracy`. These are the highest-priority fixes.

---

## Code of Conduct

By contributing, you agree to abide by the [Code of Conduct](CODE_OF_CONDUCT.md).
