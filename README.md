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

**Models Evaluated**  
Several models were tested during the summarization experiments:
- Falconsai/medical_summarization (T5-based)
- google/flan-t5-large  
- Llama 3.1 8B (via Ollama) — selected as the primary model

Llama 3.1 8B showed the best overall performance, demonstrating superior structure adherence, better coverage of available fields, and higher consistency compared to the earlier models. Falconsai and Flan-T5 frequently failed to follow the required output format.
**Setup**  
- **Primary Model**: Llama 3.1 8B (via Ollama)  
- **Samples**: 10 synthetic patient profiles  
- **Approach**: Strict structured prompting + manual comparison against QA reference summaries

**Key Results**  
- Average score: 3.8/5  
- Hallucinations: 0%  
- Factual errors: 10% (mostly inherited from data issues)  
- Formatting inconsistency: 30% (e.g., inconsistent removal of appended digits from names)

**Conclusion**: The model shows high reliability (no hallucinations) and good coverage of available fields. Limitations primarily stem from input data quality. With data cleaning and prompt refinement, performance can reach 4.5+ consistently.

See detailed evaluation: [02_summary_evaluation.ipynb](notebooks/02_summary_evaluation.ipynb)

**Artifacts**  
- `patients_samples1.csv` — file with 10 randomly selected samples from the full list

## Phase 3: Automated Evaluation & Judge Model Comparison

**Metrics used:** DeepEval GEval (4 custom criteria) · ROUGE-L · BERTScore  
**Judge models tested:** Qwen 2.5:3B · Qwen 2.5:7B · Mistral Nemo (all via Ollama)  
**Samples:** 10 synthetic patient profiles (same as Phase 2)

### Setup

- Framework: [DeepEval](https://github.com/confident-ai/deepeval) with custom `GEval` metrics
- All judge models run locally via Ollama (no external API calls)
- Each metric evaluated separately per patient to avoid Colab timeouts

### Evaluation Metrics

Four custom GEval metrics were defined to match the specific requirements
of structured medical summarization:

| Metric | What it checks |
|---|---|
| **Faithfulness to Raw Data** | No invented facts; all values grounded in input |
| **Summary Correctness** | Match against manual QA reference summary |
| **Summary Structure** | Proper labels, readable format for a physician |
| **Summary Completeness** | All available fields covered; missing ones marked Unknown |

Standard `FaithfulnessMetric` was tested first but proved too strict
for this structured summarization task — replaced with custom GEval criteria.

### Judge Model Comparison

| Judge Model | Faithfulness | Correctness | Structure | Completeness | Avg | Speed / metric |
|---|---|---|---|---|---|---|
| Qwen 2.5:3B | 0.0 | 0.5 | 0.0 | 0.0 | **0.13** | ~15 min |
| Qwen 2.5:7B | 0.8 | 0.7 | 0.8 | 0.7 | **0.75** | ~20+ min |
| Mistral Nemo | 0.8 | 1.0 | 0.6 | 0.8 | **0.80** | ~5 min (locally) |

### Key Findings

**Qwen 2.5:3B — unreliable judge.**  
Scored 0.0 on Faithfulness, Structure, and Completeness across all 10 patients,
flagging correctly generated content as hallucinations.
The model could not reliably follow multi-condition evaluation criteria.
Not suitable as a judge for this task.

**Qwen 2.5:7B — consistent but non-differentiating.**  
Produced identical scores for all 10 patients (0.8 / 0.7 / 0.8 / 0.7),
indicating the model evaluates the summary template rather than
individual patient content. Scores are plausible but lack granularity.

**Mistral Nemo — best overall judge.**  
Only model to produce meaningful per-metric differentiation.
Correctness = 1.0 (summaries match reference), Structure = 0.6
(correctly flagged missing `patient_name` label), Faithfulness = 0.8
(penalised for not marking absent healthcare coverage as Unknown).

**Critical limitation — score uniformity across patients.**  
All three judge models gave identical scores to all 10 patients.
This is a known LLM-as-a-Judge failure mode: the judge evaluates
format and template rather than case-specific content.
A reliable judge should produce variance across patients.

### ROUGE-L and BERTScore

Used as objective, non-LLM-dependent baselines alongside GEval.

- **ROUGE-L** captures structural overlap and detects omitted fields
- **BERTScore** measures semantic similarity to the reference summary

These provide a useful second opinion independent of judge model quality.

### Conclusion

Automated evaluation via LLM-as-a-Judge is viable, but judge model
choice is critical. Mistral Nemo is the most suitable local judge tested:
best quality, fastest speed, most meaningful scores.

The zero-variance finding (identical scores across all patients) is
the main limitation of this phase. A production-grade evaluation pipeline
would require either a larger judge model (e.g. GPT-4o mini, Claude Haiku
via API) or more atomic evaluation criteria that force the judge to inspect
individual field values rather than overall format.

Phase 3 establishes a three-layer evaluation pipeline:
statistical metrics (ROUGE-L) + semantic metrics (BERTScore) + LLM judge (Mistral Nemo).
This is a solid foundation for scaling to 50–100 records and
comparing multiple summarization models.

### Artifacts

- `Judge_scores.xlsx` — raw GEval scores from all three judge models across all 4 metrics

---

## Skills & Tools Demonstrated (updated)

- Manual & exploratory testing
- Data quality validation (missing values, duplicates, range/date checks, consistency)
- Python + pandas for data analysis & visualization
- Ollama for local LLM inference and judge evaluation
- DeepEval framework (GEval, custom metrics)
- ROUGE-L and BERTScore for automated summarization metrics
- LLM-as-a-Judge methodology and its practical limitations
- GitHub + Google Colab for development & version control
  
## How to Run

1. Open notebooks in Google Colab  
2. Load data from the `data/` folder  
3. Run cells sequentially

## Author

**Katerina Kasparova**  
QA Engineer transitioning to AI/ML & GenAI QA  
Wellington, New Zealand  
[LinkedIn](https://linkedin.com/in/katerina-kasparova) | [GitHub](https://github.com/rizzken)

Last updated: May 2026
