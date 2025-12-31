# Prompt Design for Synthetic Data Generation (Travel Comment Classification)

## 1. Purpose

This document describes the prompt design strategy used to generate synthetic YouTube comments for **travel-related comment classification**.

Synthetic data generation was required due to the absence of large-scale, publicly available, labeled Korean YouTube comment datasets suitable for fine-grained travel classification. The primary objective was **not volume maximization**, but **distributional realism** — approximating the linguistic noise, ambiguity, and contextual diversity of real-world YouTube comments.

The prompts were therefore designed to:

- Reduce stylistic over-regularization commonly observed in LLM-generated text
- Simulate colloquial Korean used in online video comments
- Produce label-consistent but semantically diverse samples

---

## 2. Task Definition

### Problem Statement

Given a YouTube comment written in Korean, classify whether the comment is **travel-related** or **non-travel-related**.

This classification acts as the **first-stage filter** in an NLP pipeline designed to convert raw YouTube buzz into structured analytical signals.

### Label Schema

| Label | Description |
| --- | --- |
| `1` | Comment is related to travel (planning, experience, destination, logistics, cost, etc.) |
| `0` | Comment is not related to travel (general opinion, humor, unrelated topics, spam, etc.) |

---

## 3. Category Schema for Data Diversification

To avoid semantic collapse and prompt-induced homogeneity, each batch of generated comments was constrained to a predefined **sub-category**.

### Travel-related Categories (`label = 1`)

- Travel planning (itinerary, preparation, budgeting)
- Destination impressions
- Transportation (flight, airport, transit)
- Accommodation
- Travel cost and currency
- Travel companions (solo, family, couple)
- Travel-related questions

### Non-travel Categories (`label = 0`)

- General video reactions
- Humor / sarcasm
- Creator-related comments
- Unrelated personal opinions
- Spam-like or low-information comments

Each generation batch was limited to **one category** to enforce topical clarity while preserving linguistic diversity.

---

## 4. Prompt Template (Excerpt)

The full prompt included repeated constraints, formatting instructions, and category-specific variations. Below is a **representative excerpt** illustrating the core structure and intent.

```
Generate 10 Korean YouTube comments.

Category: Travel Planning
Label: is_travel = 1

Requirements:
- Use casual spoken Korean as commonly found in YouTube comments
- Include slang, abbreviations, emojis, or minor typos where appropriate
- Avoid grammatically polished or essay-like sentences
- Vary sentence length and tone
- Do not explicitly mention the word "travel" unless contextually natural

Output format:
CSV with columns: comment, is_travel
Encoding: UTF-8 with BOM

```

This template was iteratively refined to reduce unnatural fluency and increase noise realism.

---

## 5. Style Constraints

To better approximate real YouTube comment distributions, the following stylistic constraints were enforced:

- Informal sentence endings and spoken expressions
- Emoji usage (sparingly, not uniformly)
- Sentence fragments and incomplete thoughts
- Mixed sentiment within a single comment
- Occasional ambiguity requiring contextual interpretation

Excessively polite, explanatory, or editorial tones were explicitly discouraged.

---

## 6. Output Format and Data Handling

All generated data followed a strict CSV schema to enable seamless ingestion into the training pipeline.

### Schema

| Column | Description |
| --- | --- |
| `comment` | Raw Korean YouTube-style comment text |
| `is_travel` | Binary label (`0` or `1`) |

### Formatting Rules

- UTF-8 with BOM encoding
- One comment per row
- No quotation marks unless required for CSV escaping

---

## 7. Iterative Refinement Strategy

Initial experiments revealed overfitting and limited generalization when using small synthetic datasets.

### Key Iterations

- Dataset size expansion: 500 → 1,000 → 2,000 samples
- Increased prompt specificity for stylistic realism
- Confidence-based sample selection:
    - Model predictions with confidence scores between **0.45–0.60** were manually reviewed
    - Selected samples were re-labeled and incorporated into training data

This hybrid strategy significantly improved classification stability while mitigating synthetic bias.

---

## 8. Trade-offs and Limitations

While synthetic data enabled rapid iteration, several limitations remain:

- Certain cultural nuances and sarcasm patterns are difficult to simulate
- Distribution shift remains possible when exposed to new content domains
- Manual intervention was required to correct ambiguous edge cases

Despite these limitations, the generated dataset proved sufficient for **trend-level analysis** and downstream sentiment modeling.

---

## 9. Design Rationale Summary

The prompt design strategy prioritized:

- Control over linguistic diversity rather than surface-level realism
- Clear label semantics over exhaustive coverage
- Practical usability within time and resource constraints

This approach positions the classifier as a **signal extraction layer**, not a semantic oracle, within a broader analytical pipeline.