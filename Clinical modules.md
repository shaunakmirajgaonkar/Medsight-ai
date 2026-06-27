# Clinical Modules — MedInsight AI

MedInsight AI produces six categories of clinical output from a single structured JSON response.

---

## 1. 🧬 Differential Diagnosis

**JSON key:** `differentials`

Returns a ranked list of possible diagnoses with:

| Field | Type | Description |
|---|---|---|
| `condition` | string | Diagnosis name |
| `probability` | `High / Moderate / Low` | Relative likelihood |
| `confidence_percent` | 0–100 | Model confidence score |
| `reasoning` | string | Clinical rationale |
| `icd10` | string | ICD-10 code |
| `key_features` | list | Features supporting this diagnosis |
| `against` | string | What argues against this diagnosis |

**UI:** Ranked cards with confidence bar, probability badge, ICD-10 chip, key feature tags.

---

## 2. 🔬 Lab Interpretation

**JSON key:** `lab_analysis`

Interprets every reported lab value:

| Field | Type | Description |
|---|---|---|
| `parameter` | string | Test name |
| `value` | string | Reported result with units |
| `reference` | string | Normal reference range |
| `status` | `CRITICAL / HIGH / LOW / NORMAL` | Flagging |
| `interpretation` | string | Clinical significance |
| `action_needed` | boolean | Whether immediate action is required |

**UI:** Grouped by action_needed — "Requires Action" vs "Within Reference". Color-coded by status.

---

## 3. 📊 Risk Prediction

**JSON key:** `risk_predictions`

Calculates disease and complication risk:

| Field | Type | Description |
|---|---|---|
| `condition` | string | Complication or disease |
| `risk_percent` | 0–100 | Estimated risk percentage |
| `timeframe` | string | e.g. "10-year", "5-year" |
| `basis` | string | Evidence basis (Framingham, UKPDS, ADA) |
| `modifiable` | boolean | Whether risk can be reduced |

**UI:** Grouped by modifiable vs non-modifiable. Visual risk bar (green < 35% < orange < 65% < red).

---

## 4. 💊 Drug Interactions

**JSON key:** `drug_interactions`

Checks listed medications for interactions:

| Field | Type | Description |
|---|---|---|
| `drugs` | list | Drug pair involved |
| `severity` | `Major / Moderate / Minor` | Interaction severity |
| `mechanism` | string | PK/PD explanation |
| `effect` | string | Clinical consequence |
| `action` | string | Recommended clinical action |
| `monitoring` | string | Parameters to monitor |

**UI:** Color-coded cards (red = Major, orange = Moderate, blue = Minor). Returns "No interactions identified" with green badge if none found.

---

## 5. 📋 Recommendations

**JSON key:** `recommendations`

Evidence-graded clinical actions:

| Field | Type | Description |
|---|---|---|
| `priority` | `URGENT / HIGH / MODERATE / ROUTINE` | Action priority |
| `category` | `Investigation / Medication / Referral / Monitoring / Lifestyle` | Action type |
| `action` | string | Specific actionable step |
| `evidence` | string | Guideline or study citation |
| `evidence_grade` | `A / B / C` | Evidence strength |

**Evidence grades:**
- **A** — Strong randomised controlled trial (RCT) evidence
- **B** — Observational / cohort study evidence
- **C** — Expert consensus / guideline opinion

**UI:** Grouped by category. Priority badge + grade badge on each card.

---

## 6. 📅 Follow-up Plan

**JSON key:** `follow_up`

| Field | Type | Description |
|---|---|---|
| `timeframe` | string | Recommended follow-up interval |
| `monitoring` | list | Parameters to track |
| `red_flags` | list | Symptoms requiring immediate review |

**UI:** Shown in the summary row header. Red flags highlighted in orange.

---

## System Prompt Design

The clinical analysis is driven by a structured system prompt that:
1. Defines the model's role as a decision support tool (not a diagnostic replacement)
2. Specifies guideline sources to cite (AHA/ACC, ADA, NICE, ESC, WHO)
3. Mandates differential (not definitive) diagnoses
4. Specifies the evidence grading system
5. Requires exact JSON schema output with no preamble or markdown
