# ARYA AI Design Principles

**Version:** 1.0
**Status:** Draft (Frozen after approval)

---

# Purpose

This document defines the fundamental architectural principles of ARYA AI.

These principles are intended to remain stable throughout the lifetime of the project and serve as the foundation for every architectural and implementation decision.

Whenever a new component is introduced, it must comply with these principles unless an Architecture Decision Record (ADR) explicitly states otherwise.

---

# 1. Modularity First

ARYA is designed as a collection of independent modules rather than a single monolithic application.

Each module has one clearly defined responsibility and communicates with other modules through stable contracts.

Examples include:

* Knowledge Acquisition Platform (KAP)
* Unified Repository
* Dataset Intelligence Engine
* Dataset Router
* Builder Framework
* Judge Framework
* Training Platform
* Runtime Platform

A module must never assume the internal implementation of another module.

---

# 2. Single Responsibility

Every component should have one primary responsibility.

Examples:

* KAP acquires knowledge.
* Repository stores knowledge.
* Dataset Intelligence Engine analyzes datasets.
* Dataset Router routes datasets.
* Builders generate training datasets.
* Judges validate generated datasets.
* Trainer performs model training.

No component should perform responsibilities that belong to another.

---

# 3. Repository-Centered Architecture

The Unified Repository is the single source of truth.

All validated knowledge must eventually enter the Repository before being used anywhere else.

No training pipeline may consume external data directly.

All datasets must originate from Repository objects.

---

# 4. Deterministic Before AI

ARYA always prefers deterministic algorithms whenever possible.

Examples:

* schema detection
* duplicate detection
* language detection
* metadata extraction
* license validation

Artificial intelligence should only be used where deterministic solutions cannot reasonably solve the problem.

This reduces cost, increases reproducibility and improves explainability.

---

# 5. AI Assists — AI Does Not Control

Large language models are assistants.

They provide analysis, suggestions and semantic interpretation.

They do not make final architectural or routing decisions.

Final decisions must remain deterministic whenever possible.

---

# 6. Layered Intelligence

Knowledge processing must happen progressively.

Typical flow:

Import

↓

Parsing

↓

Structural Analysis

↓

Statistical Analysis

↓

Semantic Analysis

↓

Capability Inference

↓

Dataset Descriptor

↓

Routing

↓

Dataset Builders

↓

Judging

↓

Training

Every layer enriches information produced by the previous one.

No layer should duplicate another layer's responsibility.

---

# 7. Source Agnostic Design

ARYA must never depend on where information originates.

Knowledge may come from:

* websites
* documents
* books
* datasets
* APIs
* manual uploads
* future connectors

Once imported, every source is treated equally.

Routing decisions are based only on analyzed metadata and descriptors.

---

# 8. Unified Knowledge Flow

Knowledge should enter the platform only once.

After validation, the same knowledge may be reused for multiple purposes:

* Pretraining
* Knowledge extraction
* SFT
* DPO
* Future training methods

Duplicate processing should be avoided whenever possible.

---

# 9. Separation of Learning Stages

Different learning stages solve different problems.

## Pretraining

Learns language.

## Supervised Fine Tuning (SFT)

Learns behavior.

## Direct Preference Optimization (DPO)

Learns preference.

These stages must remain independent.

They may consume knowledge from the same Repository but should never be treated as identical processes.

---

# 10. Contracts Over Implementation

Modules communicate through contracts rather than implementation details.

Every public component should define:

* inputs
* outputs
* events
* state transitions
* error handling

Implementations may change.

Contracts should remain stable.

---

# 11. Explainability

Every important decision should be explainable.

Examples include:

* Why was a dataset routed to Conversation Builder?
* Why was a document rejected?
* Why did a Judge assign a low score?

Architectural transparency is preferred over opaque automation.

---

# 12. Reproducibility

Given identical inputs, configuration and software versions, ARYA should produce identical outputs whenever deterministic processing is used.

Training datasets, checkpoints and experiments should remain reproducible.

---

# 13. Progressive Automation

Automation should increase gradually.

The preferred order is:

1. Manual
2. Rule-based
3. Statistical
4. AI-assisted
5. Autonomous

Each level must be validated before advancing to the next.

---

# 14. Extensibility

Every subsystem should allow future expansion without architectural redesign.

Examples:

* new Builders
* new Judges
* new model providers
* new dataset types
* multimodal learning
* reinforcement learning
* agent training

Future capabilities should integrate by extension rather than modification.

---

# 15. Quality Before Quantity

Higher-quality datasets are preferred over larger datasets.

Every dataset should pass validation before becoming part of the learning pipeline.

Poor-quality information should be rejected rather than propagated.

---

# 16. Human Oversight

The platform should support automation without eliminating human control.

Developers must always be able to inspect:

* routing decisions
* quality scores
* capability inference
* training datasets
* experiment history

Critical decisions should remain auditable.

---

# Summary

The principles defined in this document are the architectural foundation of ARYA AI.

All future blueprint documents—including the Knowledge Acquisition Platform, Dataset Intelligence Engine, Dataset Router, Builder Framework, Judge Framework and Training Platform—must follow these principles unless superseded by an approved Architecture Decision Record (ADR).
