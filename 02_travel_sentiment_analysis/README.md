# Sentiment Analysis on Travel-Related Comments

## Overview

This module performs **sentiment analysis exclusively on travel-related comments** filtered by the previous classification stage. The objective is not merely to label sentiment, but to **extract interpretable demand signals** embedded in user discourse.

Unlike generic sentiment tasks, travel-related comments frequently contain **neutral, information-seeking expressions** that carry latent intent. Capturing this nuance was central to the design of this module.

---

## Problem Definition

### Why Sentiment Matters Beyond Comment Volume

Initial exploration showed that raw engagement metrics (comment counts, likes) were insufficient to explain downstream travel demand.

Key observations:

- High comment volume without sentiment separation produced noisy signals
- Negative sentiment spikes often aligned with demand drops
- **Neutral comments (questions, factual statements)** preceded increases in actual travel demand

Therefore, sentiment analysis was introduced to **disaggregate travel buzz into meaningful behavioral signals**.

---

## Data Construction

### Initial Binary Sentiment Attempt

- Dataset: ~3,000 LLM-generated comments
- Labels: Positive / Negative (1:1 ratio)
- Model performance was unstable and inconsistent

Issue identified:

> Excluding neutral sentiment forces ambiguous comments into polar classes, degrading classification reliability.
> 

---

### Multi-Class Sentiment Redesign

To address this, the task was redefined as a **3-class classification problem**.

- Total samples: 4,800
    - Positive: 1,600
    - Negative: 1,600
    - Neutral: 1,600

Neutral comments were carefully designed to include:

- Information requests
- Location or facility descriptions
- Non-evaluative factual statements

This redesign significantly improved interpretability and stability.

The full prompt design rationale and representative templates for sentiment-controlled data generation are documented in the following file:

[Prompt Design File](prompts/prompt_design.md)

---

## Model Architecture

- Base model: `monologg/koelectra-base-discriminator`
- Task: Multi-class sequence classification (3 labels)
- Tokenization: KoELECTRA tokenizer
- Fine-tuning: Hugging Face's Trainer API
- Training duration: ~140 minutes

The trained sentiment classification model is publicly available on Hugging Face:

→ https://huggingface.co/USERNAME/travel-sentiment-classifier

---

## Performance Summary

Final evaluation results on the synthetic validation set:

- **Accuracy:** ~96%
- **Macro F1 Score:** ~96%

While these metrics indicate strong performance under controlled validation conditions, qualitative inspection on real-world YouTube comments revealed reduced reliability at the **individual comment level**.

This discrepancy is attributed to:
- Semantic ambiguity in short-form user comments
- Distributional differences between synthetic training data and real-world discourse
- The inherent subjectivity of sentiment interpretation in travel-related contexts

Accordingly, this model is positioned as a **trend-level signal extractor**, rather than a tool for precise per-comment sentiment judgment.

---

## Key Insight

> Neutral sentiment is not noise; it represents latent travel intent.
> 

Empirical analysis revealed that:

- Increases in neutral sentiment often preceded rises in actual travel demand
- Question-heavy comment sections function as early-stage planning signals
- Sentiment separation provided clearer leading indicators than raw volume metrics

This insight reframed sentiment analysis from a descriptive task into a **predictive signal extraction tool**.

---

## Output and Usage

- Output labels:
    - `0`: Negative
    - `1`: Neutral
    - `2`: Positive

Sentiment ratios were aggregated at the **city–time level** and used as explanatory variables in downstream demand analysis.

---

## Limitations and Future Work

- Confidence-based relabeling was planned but not fully executed due to time and hardware constraints
- Potential extensions include:
    - Active learning on raw YouTube comments
    - Context-aware sentiment using longer comment threads
    - Cross-lingual sentiment generalization

---

## Role and Contribution

This module was developed as part of a 4-member team project.

- Prompt engineering and dataset construction: Team member GY Yu
- Model framing, redesign to multi-class sentiment, evaluation strategy, and interpretation: **Author**

All outputs were jointly reviewed and integrated into the final pipeline.
