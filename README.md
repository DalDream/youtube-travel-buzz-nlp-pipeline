# YouTube Travel Buzz NLP Pipeline

## Project Identity

**An NLP pipeline that transforms YouTube travel buzz into quantifiable demand signals**

This project designs an interpretable NLP pipeline that converts unstructured YouTube travel comments into **structured, analyzable signals** that can be used for downstream travel demand analysis.

Rather than treating raw engagement volume as demand, the pipeline focuses on **semantic filtering and sentiment structuring** to extract meaningful early-stage interest signals.

---

## Project Overview

- **Period**: May 28, 2025 – June 11, 2025
- **Context**: 4-member team project conducted during a data analysis bootcamp
- **Roles**:
    - **Author (Lead):**
        - Pipeline architecture and module integration
        - Model selection and baseline training
        - Error analysis and iterative improvement strategy
        - Final documentation and validation
    - **Teammate (GY Yu)**:
        - LLM-based prompt engineering for synthetic data generation
        - Dataset construction and augmentation
        - Model training and fine-tuning
---

## Why This Pipeline Exists

YouTube travel content often captures **pre-demand signals**—questions, curiosity, and tentative interest—*before* actual travel behavior materializes.

However, conventional buzz metrics suffer from structural noise:

- Comment volume mixes travel intent with unrelated reactions
- Sentiment polarity alone fails to distinguish saturated demand from emerging interest

This pipeline addresses the problem by **engineering demand-ready signals**, not by maximizing model performance or producing descriptive analytics.

---

## Core Pipeline

```
Raw YouTube Comments
        ↓
Travel-related Comment Classification
        ↓
Sentiment Classification(Positive / Neutral / Negative)
        ↓
Structured Travel Buzz Signals
        ↓
External Demand Validation(Non-public)

```

Each stage incrementally removes noise and increases semantic alignment with real-world travel intent.

---

## Models (Hugging Face)

The core classification models used in this pipeline are publicly available on Hugging Face.

- **Travel-related Comment Classifier**
    
    https://huggingface.co/DalDream/youtube-travel-buzz-relevance-classifier
    
- **Travel Sentiment Classifier (Positive / Neutral / Negative)**
    
    https://huggingface.co/DalDream/youtube-travel-buzz-sentiment-classifier
    

Each model functions as a **signal extraction component** within the pipeline rather than a standalone analytical endpoint.

Detailed training strategy, prompt design, and model limitations are documented in the corresponding module READMEs.

---

## Key Insight (Signal Design)

Initial intuition suggested that **positive sentiment volume** would correlate with higher travel demand.

Downstream validation revealed a different pattern:

- High positive sentiment concentrated in already-saturated, major destinations
- Weak correlation between buzz volume and demand in those cases
- **Neutral sentiment comments**—primarily information-seeking questions—were more prevalent in emerging destinations
- In these contexts, neutral sentiment ratios showed **stronger leading correlation** with subsequent demand changes

This insight emerged *after* pipeline execution and directly informed **signal definition**, not retrospective storytelling.

---

## Repository Scope

### Included

- Travel-related comment classification module
- Travel sentiment classification module
- Prompt design and synthetic data generation strategy
- Public-facing Python notebooks reorganized for readability
- Model stability metrics sufficient for signal extraction

### Not Included

- Airline passenger volume data (non-public)
- Tableau dashboards used for external validation
- Correlation computation code tied to proprietary datasets

External validation is referenced only to contextualize **why the pipeline exists**, not to reproduce results.

---

## Project Structure

```
youtube-travel-buzz-nlp-pipeline/
│
├── README.md                       
│
├── 01_travel_comment_classification/
│   ├── README.md                   
│   ├── notebooks/
│   │   └── travel_comment_classifier.ipynb
│   └── prompts/
│       └── prompt_design.md
│
├── 02_travel_sentiment_analysis/
│   ├── README.md
│   ├── notebooks/
│   │   └── travel_sentiment_classifier.ipynb
│   └── prompts/
│       └── prompt_design.md
│
└── .gitignore 
```

---

## Design Philosophy

- Models are **means**, not outcomes
- Prompt design is a **data quality control layer**
- Sentiment classification is a **signal structuring step**, not an end analysis
- Interpretability and modularity take precedence over benchmark optimization

---

## Limitations

- Reliance on LLM-generated synthetic training data
- Limited fine-tuning due to time and compute constraints
- External demand validation not reproducible with public data alone

These constraints do not affect the **conceptual validity** of the signal engineering pipeline.

---

## Intended Use

This repository is designed as:

- A reference architecture for transforming social media buzz into demand-aligned features
- A foundation for future real-time or multi-platform demand sensing systems

---

## Disclaimer

Some downstream validation referenced in this project relies on non-public datasets and tools.


This repository intentionally contains **only components that can be publicly shared**, with emphasis on pipeline design rather than result disclosure.


