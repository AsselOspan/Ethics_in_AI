# SocioTable-KZ: Privacy-Preserving Generation from Sociological Survey Data

This repository accompanies the study **“SocioTable-KZ: A Privacy-Preserving Methodology for Table-to-Text Generation from Sociological Survey Data.”**

SocioTable-KZ supports the preparation of bilingual Kazakh–Russian sociological survey records for structured text generation and safety-aware analysis. The workflow covers relational-data preparation, construction of model-ready JSON/JSONL records, QLoRA-oriented training pairs, and three-level safety annotation:

- **Safe** — may be released without substantive modification;
- **Sensitive** — requires contextualisation, generalisation, or privacy-preserving rewriting;
- **Unsafe** — must not be released automatically.

> **Important privacy notice**
>
> The current project contains respondent-level sociological data. Files that include `user_id`, `user_name`, `user_phone`, `user_age`, or `user_region` must **not** be uploaded to a public GitHub repository. They are listed below only to document the local research workflow. Public release requires documented legal approval, ethics review where applicable, and confirmation that all direct and indirect identifiers have been removed or sufficiently protected.

---

## Repository Contents

```text
.
├── data/
│   ├── public/
│   │   ├── kazakh_target.jsonl
│   │   ├── russian_target.jsonl
│   │   └── russian_only.jsonl
│   └── private/                         # Do not upload to public GitHub
│       ├── kazakh_classified.jsonl
│       ├── kazakh_unique.json
│       ├── russian_classified.jsonl
│       └── russian_unique.json
├── notebooks/
│   └── 01_prepare_structured_data.ipynb
├── README.md
└── LICENSE                              # Add after selecting terms for code and data
```

The original notebook file is named `struct (2).ipynb`. For reproducibility and clearer version control, rename it to:

```text
notebooks/01_prepare_structured_data.ipynb
```

---

## Data Files

| File | Records | Main fields | Recommended release status |
|---|---:|---|---|
| `kazakh_classified.jsonl` | 1,347 | Survey metadata, respondent-linked fields, `answer_final`, `Safety_Status` | **Private only** |
| `kazakh_target.jsonl` | 1,014 | `question_title`, `answer_final`, `Safety_Status`, `target_text` | Review before public release |
| `kazakh_unique.json` | 1,198 | Survey metadata and respondent-linked fields | **Private only** |
| `russian_classified.jsonl` | 3,376 | Survey metadata, respondent-linked fields, `answer_final`, `Safety_Status` | **Private only** |
| `russian_only.jsonl` | 491 | `question_title`, `answer_final`, `target_text`, `Safety_Status` | Review before public release |
| `russian_target.jsonl` | 2,772 | `question_title`, `answer_final`, `Safety_Status`, `target_text` | Review before public release |
| `russian_unique.json` | 6,229 | Survey metadata and respondent-linked fields | **Private only** |
| `struct (2).ipynb` | — | SQLite import, SQL joins, cleaning, and JSON export | Public after removal of private paths and credentials |

### Safety-label distribution

| Language subset | Safe | Sensitive | Unsafe | Total |
|---|---:|---:|---:|---:|
| Kazakh classified subset | 292 | 1,011 | 44 | 1,347 |
| Russian classified subset | 792 | 2,536 | 48 | 3,376 |

The label distributions describe the annotated corpus. They must not be interpreted as classification-performance metrics.

---

## Data Schema

### Training and target files

The intended model-ready files use the following fields:

```json
{
  "question_title": "Survey question",
  "answer_final": "Normalised respondent answer",
  "Safety_Status": "Safe | Sensitive | Unsafe",
  "target_text": "Reference analytical text"
}
```

These fields are used to construct instruction–response examples for bilingual QLoRA adaptation and for safety-aware text-generation experiments.

### Private respondent-level files

The following fields occur in local-only records and must not be published without a formal de-identification and release review:

```text
user_id
user_name
user_phone
user_age
user_region
```

Even when a value appears pseudonymised, encoded, or incomplete, it may act as a direct identifier or quasi-identifier when combined with survey year, topic, question, answer, or other metadata.

---

## Privacy-Preserving Release Procedure

Before publishing any dataset file, complete the following checklist:

1. Remove direct identifiers, including internal respondent IDs, names, phone numbers, addresses, and traceable account labels.
2. Evaluate quasi-identifiers such as exact age, locality, year, occupation, ethnicity, and rare response combinations.
3. Check free-text answers and generated `target_text` fields for identifying, sensitive, or harmful information.
4. Confirm that public release is consistent with the consent, data-sharing agreement, institutional governance rules, and applicable legislation.
5. Publish only the minimum data needed to reproduce the reported experiments.
6. Include a dataset licence or terms of use and state whether commercial use, redistribution, or model training is permitted.
7. Do not release raw SQL dumps, SQLite databases, API keys, passwords, or private experiment logs.

A public GitHub repository should contain only approved, de-identified derivatives. Raw microdata should remain in a protected institutional environment or be made available only through a controlled-access procedure.

---

## Reproducibility

The preparation notebook imports a local SQL dump into SQLite, joins related survey tables, normalises answer fields, and exports structured JSON records. It requires local access to private source data and therefore cannot be run from a public clone without authorised data access.

### Environment

```bash
python >= 3.10
pandas
sqlite3
jupyter
```

Install the basic dependencies:

```bash
pip install pandas jupyter
```

### Local workflow

1. Obtain authorised access to the protected source database.
2. Store `dump.sql` and `survey.db` outside the public repository.
3. Open `notebooks/01_prepare_structured_data.ipynb`.
4. Run the SQL import, join, cleaning, and JSON-export cells locally.
5. Generate only approved de-identified derivatives for public release.

---

## Ethical Use

This repository is intended for research on privacy-aware processing of sociological survey data. It must not be used to:

- identify, profile, or make decisions about individual respondents;
- infer sensitive attributes about identifiable individuals;
- reproduce or disclose respondent-level microdata;
- create discriminatory, stigmatising, or harmful content;
- bypass consent, governance, or controlled-access requirements.

Outputs classified as **Sensitive** should be generalised or neutrally reformulated before release. Outputs classified as **Unsafe** should be blocked from automatic delivery and reviewed under an approved governance procedure.

---

## Citation

Please update the citation once the article has a DOI or final bibliographic details.

```bibtex
@article{ospan2026sociotablekz,
  title   = {SocioTable-KZ: A Privacy-Preserving Methodology for Table-to-Text Generation from Sociological Survey Data},
  author  = {Ospan, Assel and Mansurova, Madina and Zhangabay, Zhansaya and Zhappar, Aktoty},
  year    = {2026},
  note    = {Manuscript under preparation}
}
```

---

## Licence

No licence is granted by this repository until explicit licence files are added.

Before release, specify separately:

- a licence for source code;
- data terms of use for public de-identified files;
- any restrictions required by the underlying data-sharing agreement.

---

## Contact

For questions about the project, authorised access, or responsible data use, contact the corresponding author through the contact details provided in the associated manuscript.
