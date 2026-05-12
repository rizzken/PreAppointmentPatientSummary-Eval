# PreAppointmentPatientSummary-Eval

**LLM-based patient profile summarization — data quality + automated evaluation pipeline**

## Project Overview

This project evaluates the quality of synthetic patient data and tests how well generative AI models can create concise, structured summaries for physicians before a first appointment.

**Goal**
- Identify data quality issues in patient records
- Test LLM summarization capabilities (structure, faithfulness, consistency)
- Explore manual vs automated evaluation approaches (including LLM-as-a-Judge)
- Demonstrate a full-cycle GenAI testing methodology

The project is inspired by a real need in primary care - doctors need fast and accurate patient overviews.

## Skills & Tools Demonstrated

- Data Quality Analysis (pandas)
- Manual LLM evaluation
- Prompt engineering
- Ollama (local LLMs)
- DeepEval + custom G-Eval metrics
- ROUGE-L, BERTScore
- LLM-as-a-Judge methodology and its limitations
- End-to-end testing pipeline for GenAI applications

## Phase 1: Data Quality Assessment

Tested on 115 synthetic patient records.

**Key Findings**
- High percentage of missing values (suffix 99%, deathdate 87%, etc.)
- Invalid dates, negative values, duplicates and formatting inconsistencies
- Even synthetic data requires thorough validation before LLM usage

**Artifacts**: `patients_with_age.csv`

## Phase 2: LLM Summarization & Manual Evaluation

**Generator**: Llama 3.1 8B (via Ollama)  
**Samples**: 10 patients  
**Approach**: Strict structured prompt + manual QA evaluation

**Key Results**
- Average score: **3.8/5**
- Hallucinations: **0%**
- Main issue: formatting inconsistency (30–40%)

[Detailed evaluation](notebooks/02_summary_evaluation.ipynb)

## Phase 3: Automated Evaluation & Judge Model Comparison

**Metrics**: Custom DeepEval G-Eval + ROUGE-L + BERTScore  
**Judge models**: Qwen2.5 3B, Qwen2.5 7B, **Mistral Nemo**

**Best result** (Mistral Nemo):
- Average automated score: **0.80**
- Fastest and most reliable judge when run locally

**Important finding**: Smaller judge models can be unstable or overly strict. Choice of judge model is critical in LLM-as-a-Judge approach.

[Detailed Phase 3](notebooks/03_automated_evaluation.ipynb)

## Conclusion

This project demonstrates a complete testing cycle for an LLM-based medical summarization feature - from data quality to manual evaluation and automated metrics. It highlights both the potential and current limitations of using generative AI in healthcare contexts.

Particularly valuable experience was gained in building evaluation pipelines and understanding the nuances of LLM-as-a-Judge methodology.

---

## How to Run
1. Open notebooks in Google Colab
2. Load data from the `data/` folder
3. Run cells sequentially

**Author**: Katerina Kasparova  
**QA Engineer transitioning to AI/ML & GenAI QA**  
  
[LinkedIn](https://linkedin.com/in/katerina-kasparova) | [GitHub](https://github.com/rizzken)

Last updated: May 2026
