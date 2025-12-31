# Travel Comment Classification

## Overview

This module focuses on building a binary text classification model to determine whether a YouTube comment is **travel-related** or **non-travel-related**. This step is the foundation of the entire pipeline, as only comments classified as travel-related are passed downstream for sentiment analysis and demand signal interpretation.

The core challenge addressed here is that **keyword-based filtering fails to capture contextual travel intent**, especially in noisy, colloquial YouTube comments. Therefore, a context-aware language model was required.

---

## Problem Definition

### Why Travel Comment Classification Matters

YouTube travel videos attract a wide range of comments, including:

- Editing or video quality feedback
- Reactions unrelated to travel intent
- Casual chat or spam

If these comments are not filtered out, downstream metrics such as comment volume or sentiment ratios become distorted and unreliable.

The objective of this module is to:

- Accurately identify comments that contain **explicit or implicit travel context**
- Minimize false positives that inflate perceived travel interest

---

## Data Construction

### Initial Approach and Limitations

- Training data was initially generated using LLMs (approximately 500 samples)
- Early experiments showed **severe overfitting** and low generalization

Key issue identified:

> LLM-generated comments were grammatically clean and stylistically uniform, deviating from real-world YouTube comment noise.
> 

---

### Data Expansion and Prompt Engineering

To address this gap, the dataset was progressively refined:

- Expanded dataset size: 500 → 1,000 → 2,000 comments
- Balanced class distribution (travel / non-travel)
- Introduced detailed prompt constraints to simulate:
    - Slang, typos, emojis, repetition
    - Mixed sentence lengths and tones
    - Realistic ambiguity seen in actual comment sections

This significantly improved model robustness.

The full prompt design rationale and representative templates used for generating realistic linguistic noise and semantic diversity are documented in the following file:

[Prompt Design File](prompts/prompt_design.md)

---

## Model Architecture

- Base model: `monologg/koelectra-small-discriminator`
- Task: Binary sequence classification
- Tokenization: Standard KoELECTRA tokenizer
- Fine-tuning: Hugging Face's Trainer API
- Train/validation split: 80% / 20%

The trained model for this module is publicly available on Hugging Face:

→ https://huggingface.co/USERNAME/travel-comment-classifier

---

## Human-in-the-Loop Enhancement

### Confidence-Based Error Analysis

Despite improved data quality, the model still showed uncertainty in borderline cases.

Strategy applied:

- Extracted predictions with confidence scores between **0.45–0.60**
- These samples represent ambiguous cases where the model struggled most

### Manual Relabeling

- Approximately 200 comments were manually reviewed and relabeled
- These samples were reintegrated into the training set
- The model was retrained with this augmented dataset

This approach mirrors practical **active learning / human-in-the-loop** workflows used in production systems.

---

## Performance Summary

After data refinement and confidence-based retraining:

- **Accuracy:** ~95%
- **F1 Score:** ~95%

While early-stage metrics were not systematically logged, qualitative comparison confirmed substantial robustness gains after data refinement.

> The primary goal was not metric maximization, but stability under noisy real-world inputs.
> 

---

## Key Insight

> High-quality data design matters more than model complexity.
> 

This experiment demonstrated that:

- Simply increasing data volume is insufficient
- Prompt design and ambiguity-aware correction are critical
- Confidence-based human intervention is a scalable and effective strategy

---

## Output and Usage

- Output: Binary label (`is_travel = 1 / 0`)
- Only comments classified as `is_travel = 1` are forwarded to the sentiment analysis module

This ensures that downstream demand signals are derived from **semantically valid travel discourse**, not raw engagement noise.

---

## Limitations and Future Work

- Hyperparameter tuning was not performed due to time and hardware constraints
- Future improvements could include:
    - Semi-supervised learning with raw comments
    - Dynamic thresholding for confidence selection
    - Cross-domain validation on non-travel YouTube channels

---

## Role and Contribution

This module was developed as part of a 4-member team project.

- Prompt design and data generation: Team member GY Yu
- Model selection, training strategy, error analysis, and final validation: **Author**

All outputs were jointly reviewed and consolidated.