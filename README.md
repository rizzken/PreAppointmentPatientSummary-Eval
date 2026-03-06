# PreVisitPatientSummary-Eval

Quality evaluation of patient demographic summarization before primary care visit

## Project Overview

This project evaluates the quality of synthetic patient demographic data and tests the ability of generative AI models to create concise, structured summaries for physicians before a first-time patient visit.

**Goal**  
- Identify common data quality issues (missing values, invalid dates, duplicates, inconsistencies)  
- Assess LLM performance on summarization tasks (coverage, hallucinations, factual accuracy, formatting)  
- Highlight real-world limitations in using GenAI for medical data preparation

The project is inspired by a real primary care need: doctors require quick, accurate patient overviews before consultations.

## Skills & Tools Demonstrated

- Manual & exploratory testing  
- Data quality validation (missing values, duplicates, range/date checks, consistency)  
- Python + pandas for data analysis & visualization  
- Ollama for LLM summarization  
- Manual evaluation (coverage, hallucinations, factual checks)  
- GitHub + Google Colab for development & version control


## Phase 1: Data Quality Assessment

Tested on 115 synthetic patient records.

**Key Findings**  
- Dataset size: 115 rows, 28 columns  
- Highest missing values: suffix (99%), deathdate (87%), maiden (73%)  
- Duplicates: 2 full row duplicates  
- Unrealistic values: 6 negative records in income/expenses  
- Invalid dates: 5 future birthdates or illogical deathdates  
- Gender inconsistencies: multiple spellings detected  

**Not included in this phase**: Check for presence of at least one identification document (ssn, drivers, passport).  
(Planned for future iteration if ID becomes relevant for summarization.)

**Conclusion**: Even synthetic demographic data contains critical issues that must be addressed before LLM summarization.

**Artifacts**  
- `patients_with_age.csv` — dataset with calculated age column (for alive patients)

## Phase 2: LLM Summarization & Evaluation

**Model**: Llama 3.1 8B (via Ollama)  
**Samples**: 10 synthetic patient profiles  
**Approach**: Strict structured prompt + manual comparison with QA reference summaries

**Key Results**  
- Average score: 3.8/5  
- Hallucinations: 0%  
- Factual errors: 10% (mostly inherited from data issues)  
- Formatting inconsistency: 30% (e.g., inconsistent removal of appended digits from names)

**Conclusion**: The model shows high reliability (no hallucinations) and good coverage of available fields. Limitations primarily stem from input data quality. With data cleaning and prompt refinement, performance can reach 4.5+ consistently.

See detailed evaluation: [02_summary_evaluation.ipynb](notebooks/02_summary_evaluation.ipynb)

**Artifacts**  
- `patients_samples1.csv` — file with 10 randomly selected samples from the full list

## How to Run

1. Open notebooks in Google Colab  
2. Load data from the `data/` folder  
3. Run cells sequentially

## Author

**Katerina Kasparova**  
QA Engineer transitioning to AI/ML & GenAI QA  
Wellington, New Zealand  
[LinkedIn](https://linkedin.com/in/katerina-kasparova) | [GitHub](https://github.com/rizzken)

Last updated: March 2026
