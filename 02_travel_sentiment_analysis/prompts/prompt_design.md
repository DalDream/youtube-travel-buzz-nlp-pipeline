# Prompt Design for Synthetic Data Generation (Sentiment Classification)

## 1. Purpose

This document describes the prompt design strategy used to generate synthetic YouTube comments for **sentiment classification of travel-related content**.

The objective of this prompt design was not to create emotionally exaggerated samples, but to **preserve subtle behavioral signals embedded in real YouTube discourse**, particularly those relevant to downstream travel demand analysis.

Given the lack of large-scale, labeled Korean datasets suitable for nuanced sentiment modeling in travel contexts, synthetic data generation was employed with a focus on:

- Capturing realistic emotional ambiguity
- Separating evaluative sentiment from intent-driven neutrality
- Avoiding stylistic over-regularization commonly produced by LLMs

To mitigate stylistic bias introduced by any single generative model, synthetic data was generated using multiple large language models.

This multi-LLM strategy was adopted to improve linguistic diversity and reduce over-regularization in sentiment expressions.

Models utilized during data generation included:
GPT, Claude, Gemini, DeepSeek, Copilot, Clova-X, Perplexity, Genspark, WRTN, and Grok.

Generated outputs were iteratively reviewed and consolidated during dataset construction.

---

## 2. Task Definition

### Problem Statement

Given a YouTube comment related to travel, classify the **sentiment expressed in the comment** as *positive*, *neutral*, or *negative*.

This task operates on comments that have already passed the **travel relevance filter**, and serves as the second stage in an NLP pipeline designed to transform YouTube buzz into structured analytical signals.

### Label Schema

| Label | Sentiment | Description |
| --- | --- | --- |
| `2` | Positive | Explicit satisfaction, praise, enjoyment, or favorable evaluation |
| `1` | Neutral | Information-seeking, factual description, or emotionally non-committal remarks |
| `0` | Negative | Explicit dissatisfaction, complaint, disappointment, or unfavorable evaluation |

---

## 3. Category Schema for Data Diversification

To prevent semantic collapse and sentiment shortcut learning, each generated comment was additionally constrained by a **sentiment-related category**, used solely for **data diversification**.

### Example Category Dimensions

- Revisit intention
- Scenic / atmosphere evaluation
- Comfort / relaxation experience
- Price and value assessment
- Service interaction
- Cleanliness / hygiene
- Crowd / noise level
- Expectation vs. reality
- Food / accommodation experience
- Transportation / accessibility

Categories were **not prediction targets**, but functioned as structural scaffolding to ensure diverse expressions within each sentiment class.

---

## 4. Prompt Template (Excerpt)

The full prompt consisted of detailed constraints and category-specific instructions.

Below is a **representative excerpt** illustrating the core structure.

```
Generate 90 Korean YouTube comments related to travel.

For each category:
- 3 negative comments (sentiment = 0)
- 3 neutral comments (sentiment = 1)
- 3 positive comments (sentiment = 2)

Requirements:
- Use casual spoken Korean typical of YouTube comments
- Allow slang, abbreviations, emojis, and minor grammatical errors
- Avoid overly polished or essay-like sentences
- Mix short reactions, questions, and descriptive remarks

Output format:
CSV with columns: comment, sentiment, category
Encoding: UTF-8 with BOM

```

The template was iteratively refined to ensure that **neutral comments remained emotionally non-evaluative**, rather than weakly positive.

---

## 5. Style Constraints

To approximate real-world YouTube comment distributions, the following stylistic constraints were enforced:

- Informal, conversational tone
- Mixed sentence length and structure
- Emotionally explicit wording for positive and negative classes
- Emotionally restrained phrasing for neutral class
- Natural inclusion of questions, observations, and factual statements

Prompts explicitly discouraged:

- Editorial explanations
- Review-style summaries
- Overly enthusiastic or exaggerated emotional language

---

## 6. Output Format and Data Handling

All generated samples adhered to a strict CSV schema to ensure compatibility with downstream training workflows.

### Schema

| Column | Description |
| --- | --- |
| `comment` | Raw Korean YouTube-style comment |
| `sentiment` | Sentiment label (`0`, `1`, `2`) |
| `category` | Sentiment category (for generation only) |

### Formatting Rules

- UTF-8 with BOM encoding
- One comment per row
- No header row
- Category field retained only during training data construction

---

## 7. Iterative Refinement Strategy

Early experiments revealed poor performance when sentiment was modeled as a binary task.

### Key Iterations

- Transition from binary → multi-class sentiment structure
- Explicit definition and expansion of the neutral class
- Dataset scaling to **4,800 samples**:
    - Positive: 1,600
    - Neutral: 1,600
    - Negative: 1,600

Additional neutral-only prompts were introduced to strengthen class boundaries and reduce misclassification.

---

## 8. Trade-offs and Limitations

While synthetic data enabled rapid experimentation, several limitations remain:

- Neutral sentiment is inherently ambiguous and difficult to model
- Subtle sarcasm and cultural nuance are not fully captured
- Confidence-based fine-tuning on real comments was planned but not completed due to time and resource constraints

Nevertheless, the dataset proved sufficient for **relative comparison and trend-level analysis**.

---

## 9. Design Rationale Summary

The sentiment prompt design prioritized:

- Intent preservation over emotional intensity
- Balanced class distributions to prevent sentiment dominance
- Clear separation between evaluation and information-seeking behavior

This design decision directly supported the broader analytical goal:

> Treating sentiment classification as a signal decomposition layer, not an isolated NLP task.
>