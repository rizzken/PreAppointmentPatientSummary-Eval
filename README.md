# PreSurgicalNotes-Eval

Quality evaluation of pre-surgical medical notes summarization

## Project Overview

This project evaluates the quality of synthetic pre-surgical medical discharge notes and assesses the performance of generative AI models in summarizing them for surgeons' pre-operative preparation.

**Goal**  
Identify common data quality issues (missing values, duplicates, invalid entries) and weaknesses in AI-generated summaries (hallucinations, omission of critical facts, factual inconsistency) to understand the reliability of using AI for concise patient overviews.

The project is motivated by a real-world need in surgical practice: surgeons require fast, accurate summaries of a patient’s current medications, allergies, recent procedures, and key risks before an operation.

## Skills & Tools Demonstrated

- Manual & exploratory testing
- Data quality validation (missing values, duplicates, range checks, consistency)
- Python + pandas + sqlite3 for data analysis
- Hugging Face Transformers (e.g., Falconsai/medical_summarization)
- Text generation quality metrics (ROUGE, manual faithfulness evaluation)
- GitHub + Google Colab for development and version control

## Project Structure
PreSurgicalNotes-Eval/
├── data/                        # synthetic discharge notes (.csv)
├── notebooks/
│   └── 01_data_quality_check.ipynb     # initial data validation
│   └── 02_summary_evaluation.ipynb     # summarization & quality assessment
├── reports/                     # evaluation tables & findings
└── README.md


## Phase 1: Data Quality Assessment

Tested on 35 synthetic patient records.

**Key Findings** (example – update with real results):
- Missing values in "Allergies" field — 14% of cases
- Invalid age values (negative or >120) — 2 cases
- Duplicate patient IDs — 3 cases
- Inconsistent procedure dates — 1 case

**Conclusion**: Even synthetic datasets require rigorous validation before use in ML/GenAI pipelines.

## Next Steps (Planned)

- Text summarization using open-source LLMs
- Automated & manual quality evaluation of summaries (ROUGE score, faithfulness, coverage of key facts)
- Testing for hallucinations, bias, and nondeterminism
- Comparison of multiple models

## How to Run

1. Open the notebook in Google Colab  
2. Copy the notebook from this repository  
3. Upload data from the `data/` folder  
4. Run cells sequentially

## Author

Katerina  Kasparova
QA Engineer transitioning to AI/ML QA  
[LinkedIn] (linkedin.com/in/katerina-kasparova)
