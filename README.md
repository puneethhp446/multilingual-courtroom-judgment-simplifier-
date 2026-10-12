# Multilingual Courtroom Judgment Simplifier

A GenAI project that turns dense Indian civil court judgments into plain-language explanations, with the aim of supporting 10+ Indian languages. Built as a BCA (AI/ML specialization) capstone project.

> **Data note:** The dataset is **synthetic** (2000 civil judgments generated with an LLM). Real judgments are confidential and hard to access, so no real case data is used in this repository.

## Problem
Most people cannot understand court judgments written in dense legal English. This project extracts the key legal principle (ratio decidendi) from a judgment, explains it in simple language, and will translate the result into Indian languages with legal terms explained from a verified glossary.

## Dataset
- 2000 synthetic Indian civil judgments in CSV format
- Fields: `Case_Name`, `Facts_of_the_Case`, `Issues`, `Arguments`, `Ratio_Decidendi`, `Final_Order`, `Legal_Terms`
- No missing values or duplicates; an average case is about 525 words
- 31 unique legal terms (e.g. Bona Fide, Estoppel, Prima Facie, Res Judicata)
- Split: 1600 train / 200 validation / 200 test (fixed random seed)

## Project phases

| Phase | Work | Status |
|---|---|---|
| 1 | Data cleaning, train/val/test split, baseline LLM summarizer | Done |
| 2 | Ratio decidendi extraction, scoring, plain-language rewriting | Done (first version) |
| 3 | Legal-term glossary with retrieval (RAG), translation into Indian languages | Planned |
| 4 | Hallucination guard, final evaluation, demo app | Planned |

## Results so far

**Phase 1: baseline summarizer** (Qwen2.5-3B-Instruct, 20 test cases, manual review of 4)
- Readable plain-language summaries with mostly correct outcomes
- Loses legal distinctions and figures, and sometimes misreads orders (for example "no order as to costs")

**Phase 2: ratio decidendi extraction** (20 test cases, scored against the dataset's reference ratio)

| Version | ROUGE-1 | ROUGE-L | BERTScore F1 |
|---|---|---|---|
| v1: plain prompt | 0.402 | 0.208 | 0.844 |
| v2: two worked examples | 0.397 | 0.229 | 0.858 |

- Few-shot examples gave a small improvement in meaning similarity (BERTScore), and made the output read more like a real ratio
- The model can state incorrect law confidently, and plain-language rewriting carries such errors forward, which motivates the hallucination guard planned for Phase 4

## Planned approach
- **Grounded explanations:** a glossary of the legal terms, retrieved with embeddings (FAISS or Chroma)
- **Translation:** IndicTrans2 for Hindi, Kannada and one more language to start with
- **Hallucination guard:** flag any summary sentence that cannot be traced to the source text
- **Demo:** a simple Streamlit or Gradio app

## Tech stack
Python, Google Colab (T4 GPU), Hugging Face Transformers, Qwen2.5-3B-Instruct, ROUGE and BERTScore for evaluation

## Repository contents
- `notebooks/`: Colab notebooks for each phase
- `reports/`: phase reports

## Limitations
- The data is synthetic and uniform, so results may look better than on real judgments
- Only 20 test cases have been scored so far, so small score differences may be noise
- Plain-language outputs are judged by reading, not by a numeric score

## Author
[Puneeth HP], BCA (AI/ML specialization), [Jain university], [2026]
