# Dataset Intelligence Engine (DIE)

**Version:** 1.0
**Status:** Draft (Frozen after approval)

---

# Purpose

The Dataset Intelligence Engine (DIE) is responsible for transforming Repository objects into structured, machine-understandable Dataset Descriptors.

DIE does **not** train models.

DIE does **not** generate datasets.

DIE does **not** route datasets.

Its sole responsibility is to understand Repository objects and produce deterministic, reproducible metadata describing their characteristics.

The Dataset Descriptor produced by DIE becomes the only input for Dataset Router decisions.

---

# Design Philosophy

The Dataset Intelligence Engine follows one fundamental principle:

> **Analyze first. Route later. Build later.**

Analysis and decision making must remain separate.

The engine answers questions such as:

* What kind of document is this?
* What capabilities does it provide?
* How reliable is it?
* What learning tasks could benefit from it?

It never answers:

* Which Builder should process this?

That responsibility belongs exclusively to the Dataset Router.

---

# High-Level Architecture

```text
Repository Object
        │
        ▼
Structure Parser
        │
        ▼
Schema Detector
        │
        ▼
Rule Engine
        │
        ▼
Statistical Analyzer
        │
        ▼
Semantic Analyzer
        │
        ▼
LLM Assisted Analyzer (optional)
        │
        ▼
Capability Inference
        │
        ▼
Curriculum Analyzer
        │
        ▼
Dataset Descriptor
```

Each stage enriches the object.

No stage should repeat work already completed by previous stages.

---

# Why Multiple Analysis Layers?

Different kinds of information require different techniques.

For example:

* Detecting UTF-8 encoding is deterministic.
* Detecting a JSON schema is deterministic.
* Detecting dialogue structure is rule-based.
* Detecting topic difficulty is statistical.
* Detecting implicit instructional intent may require semantic analysis.

Using LLMs for every task is unnecessary, slower and more expensive.

Therefore analysis progresses from deterministic methods toward semantic methods only when required.

---

# Analysis Pipeline

## Stage 1 — Structure Parser

Purpose:

Read the physical representation of a Repository object.

Responsibilities include:

* format detection
* encoding validation
* parsing
* structural extraction

Outputs include:

* document format
* size
* section count
* token estimate

Parser never interprets meaning.

---

## Stage 2 — Schema Detection

Purpose:

Identify known document structures.

Examples include:

Conversation datasets

```json
messages[]
```

Preference datasets

```json
chosen
rejected
```

Question-answer datasets

```json
question
answer
```

Plain text

Markdown

Technical documentation

Schema Detection relies entirely on deterministic rules.

---

## Stage 3 — Rule Engine

Purpose:

Infer deterministic properties.

Examples:

Conversation schema

↓

contains_dialogue = true

Preference schema

↓

contains_preferences = true

Question-answer schema

↓

contains_qa = true

These rules should remain explainable and reproducible.

---

## Stage 4 — Statistical Analysis

Purpose:

Measure quantitative characteristics.

Typical metrics include:

* average sentence length
* average response length
* vocabulary diversity
* token distribution
* punctuation ratio
* code ratio
* markdown density
* dialogue depth

No semantic interpretation occurs at this stage.

---

## Stage 5 — Semantic Analysis

Purpose:

Understand document content.

Possible outputs include:

* primary topic
* domain
* instructional characteristics
* reasoning complexity
* explanation style
* task category

Semantic Analysis may use lightweight NLP models or specialized classifiers.

---

## Stage 6 — LLM Assisted Analysis

LLMs are optional.

They are used only when deterministic and statistical analysis cannot confidently classify a Repository object.

Typical tasks include:

* ambiguous intent
* hidden instructional content
* latent reasoning estimation
* conversation quality estimation
* semantic summarization

LLMs never overwrite deterministic information.

Instead they enrich existing analysis.

Every LLM-generated result must include a confidence score.

---

## Stage 7 — Capability Inference

Purpose:

Determine what learning opportunities the Repository object provides.

Examples:

Supports Pretraining

Supports Knowledge Builder

Supports Conversation Builder

Supports Preference Builder

Capability inference is performed using deterministic rules over previous analysis results.

LLMs do not perform capability inference directly.

---

## Stage 8 — Curriculum Analysis

Purpose:

Estimate learning complexity.

Typical indicators include:

* reasoning complexity
* terminology density
* document length
* dialogue complexity
* instruction difficulty

Outputs support future curriculum generation.

---

# Dataset Descriptor

The Dataset Descriptor is the final product of DIE.

Every Repository object receives exactly one Descriptor version.

The Descriptor should contain information such as:

Identity

* Repository ID
* Descriptor Version

General

* language
* document type
* source confidence
* quality score

Capabilities

* supports_pretraining
* supports_conversation
* supports_preference
* supports_knowledge

Analysis

* reasoning level
* dialogue indicators
* preference indicators
* curriculum level

Statistics

* token estimate
* vocabulary diversity
* dialogue depth

Every field should include provenance whenever possible.

---

# Confidence Scores

Not every analysis result has equal certainty.

Each inferred field should include confidence.

Example:

```yaml
reasoning_level:
  value: medium
  confidence: 0.82
```

Deterministic outputs always receive full confidence.

Semantic outputs may receive lower confidence.

Confidence values allow downstream components to make informed decisions.

---

# Interaction with Dataset Router

Dataset Router consumes Dataset Descriptors.

It never re-analyzes Repository objects.

If additional analysis becomes necessary, responsibility returns to DIE.

This separation prevents duplicated logic.

---

# Interaction with Builders

Builders never inspect Repository objects directly.

Builders consume Dataset Descriptors together with Repository references.

This guarantees consistent analysis across the platform.

---

# Performance Strategy

Analysis follows a progressive strategy.

1. Deterministic parsing
2. Rule-based inference
3. Statistical analysis
4. Semantic analysis
5. Optional LLM analysis

Expensive analysis should occur only when simpler methods cannot provide sufficient confidence.

---

# Extensibility

New analyzers should be implemented as plugins.

Examples include:

* Vision Analyzer
* Audio Analyzer
* Source Bias Analyzer
* Medical Analyzer
* Legal Analyzer
* Scientific Analyzer

Existing analyzers should remain unaffected.

---

# Summary

The Dataset Intelligence Engine is responsible for understanding Repository objects.

It transforms raw Repository data into rich, structured Dataset Descriptors through a layered analysis pipeline.

By separating analysis from routing and dataset generation, DIE ensures deterministic, explainable and reusable intelligence that supports every subsequent stage of the ARYA Learning Platform.
