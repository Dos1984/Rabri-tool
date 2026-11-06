# RABRI Tool

Structured data capture for the RABRI (Rheumatoid Arthritis BioRegistry of Ireland) follow-up visit form (v3, Aug 2020, updated 23/11/23), plus a patient pre-visit questionnaire.

Both pages are single, self-contained HTML files with no build step.

| Page | File | Who uses it |
| --- | --- | --- |
| Follow-up visit capture | `index.html` | Clinicians and research staff |
| Pre-visit questionnaire | `previsit.html` | Patients, before their appointment |

## Follow-up visit capture (`index.html`)

- Every section of the RABRI follow-up form: visit details, biometrics, employment, 1987 ACR criteria, 2010 ACR/EULAR criteria, co-morbidities, disease activity, biologic/targeted therapy, csDMARDs, corticosteroids, end of visit.
- Collapsible sections, each showing a count of completed fields and a summary.
- Follow-up questions appear only when they apply. Answers to hidden questions are not saved.
- An ⓘ pop-up on every field explains what to enter.
- Tap-to-mark 28-joint count on body and hand images.
- Live DAS28-ESR, DAS28-CRP, CDAI, SDAI and ACR/EULAR Boolean 2.0 remission. Also BMI, waist–hip ratio, the 1987 criteria count and the 2010 score.
- Dictation on free-text fields (uses the browser's speech recognition where allowed, otherwise explains how to use the device's own dictation).
- Patients are stored by RABRI ID and site only. No names or hospital numbers.
- Per-patient DAS28 trend chart, visit history, and CSV / JSON export of every field.

### Scoring

| Score | Formula | Remission | Low | Moderate | High |
| --- | --- | --- | --- | --- | --- |
| DAS28-ESR | 0.56√TJC + 0.28√SJC + 0.70 ln(ESR) + 0.014 × PtGA(mm) | <2.6 | ≤3.2 | ≤5.1 | >5.1 |
| DAS28-CRP | 0.56√TJC + 0.28√SJC + 0.36 ln(CRP mg/L + 1) + 0.014 × PtGA(mm) + 0.96 | <2.6 | ≤3.2 | ≤5.1 | >5.1 |
| CDAI | TJC + SJC + PtGA(cm) + EvGA(cm) | ≤2.8 | ≤10 | ≤22 | >22 |
| SDAI | CDAI + CRP (mg/dL) | ≤3.3 | ≤11 | ≤26 | >26 |

## Pre-visit questionnaire (`previsit.html`)

Patients answer in plain language before clinic: how they have been (free text or dictation), patient global, weight, work, new health conditions, medicine changes, steroids and questions for the visit.

1. Copy the patient's personal link from their page in the visit tool (`previsit.html#id-<RABRI ID>`) and send it to them.
2. The patient completes the form and taps **Create my reply**. They then email the generated reply text (it contains a `RABRI1:…:END` code) to the clinic.
3. Staff paste the email into the patient's page in the visit tool. This creates a visit marked *Awaiting consultation*, with the patient's answers filled in and tagged **Patient**.
4. At clinic, **Record follow-up visit** opens that visit. The clinician checks the answers, then adds the examination, bloods and scores.

The reply code is base64-encoded JSON. It is **not encrypted**, so treat the email as you would any email containing health information.

## Where data is stored

- **Inside Claude (published artifact):** records go to the artifact's shared record store, available to everyone the artifact is shared with.
- **On GitHub Pages or any static host (standalone mode):** records are saved in that browser's local storage, on that device only. A banner says so. Use **Records → Export JSON** for backups and **Import JSON backup** to restore or move data to another device. Clearing browser data deletes the records.

Standalone mode is not a shared or backed-up database. Before using it with real patient data across sites, connect a proper backend that meets your hospital's data-protection (GDPR) requirements.

## Running locally

Open `index.html` in a browser, or serve the folder:

```sh
python3 -m http.server 8000
```
