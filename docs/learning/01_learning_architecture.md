# Learning Architecture

**Version:** 1.0
**Status:** Draft (Frozen after approval)

---

# Purpose

The Learning Platform is responsible for transforming raw knowledge into high-quality training datasets and training foundation models.

Unlike traditional LLM pipelines, ARYA separates language learning from behavior learning. Every learning stage has a dedicated purpose and shares a common knowledge foundation.

The Learning Platform is built around a unified Repository and a modular processing pipeline that supports continuous expansion without architectural redesign.

---

# Design Goals

The Learning Platform is designed to achieve the following goals:

* Separate knowledge acquisition from model training.
* Reuse the same knowledge for multiple learning objectives.
* Build deterministic pipelines whenever possible.
* Support modular builders and future learning methods.
* Maintain reproducibility and traceability across every training stage.
* Allow continuous dataset evolution without modifying the training engine.

---

# High-Level Architecture

The complete learning pipeline is organized as follows:

```text
Knowledge Sources
        │
        ▼
Knowledge Acquisition Platform (KAP)
        │
        ▼
Unified Repository
        │
        ▼
Dataset Intelligence Engine (DIE)
        │
        ▼
Dataset Descriptor
        │
        ▼
Dataset Router
        │
 ┌──────┼────────┬─────────┐
 ▼      ▼        ▼         ▼
Corpus  Knowledge Conversation Preference
Builder Builder    Builder      Builder
 │        │           │             │
 └────────┼───────────┼─────────────┘
          ▼
     Judge Framework
          ▼
    Training Platform
```

Each layer has a single responsibility.

No layer should duplicate the work of another.

---

# Knowledge Sources

Knowledge may originate from multiple sources, including:

* Web crawling
* Manual uploads
* Public datasets
* Documentation
* Books
* Academic papers
* APIs
* Future connectors

Once imported, every source follows the same processing pipeline.

The origin of the data must not influence learning decisions.

---

# Knowledge Acquisition Platform (KAP)

The Knowledge Acquisition Platform is responsible for discovering, importing and validating information.

Its responsibilities include:

* Source discovery
* Connector management
* Import workflows
* Duplicate detection
* Fingerprinting
* Quality validation
* Metadata generation

KAP does not decide how knowledge will be trained.

Its responsibility ends when validated knowledge reaches the Repository.

---

# Unified Repository

The Repository is the single source of truth.

Every validated document eventually becomes a Repository object.

The Repository stores:

* Original documents
* Metadata
* Quality information
* Provenance
* Repository identifiers

Training pipelines never consume external sources directly.

All learning begins from Repository objects.

---

# Dataset Intelligence Engine

The Dataset Intelligence Engine enriches Repository objects by generating structured descriptors.

Its responsibilities include:

* Structural analysis
* Schema detection
* Statistical analysis
* Semantic analysis
* Capability inference
* Curriculum estimation

The engine does not perform routing.

Its only responsibility is producing Dataset Descriptors.

---

# Dataset Descriptor

The Dataset Descriptor represents the analyzed characteristics of a Repository object.

Typical information includes:

* language
* document type
* quality score
* confidence
* detected capabilities
* curriculum level
* reasoning complexity
* dialogue indicators
* preference indicators

Descriptors are immutable outputs of the analysis stage.

They become the primary input for routing decisions.

---

# Dataset Router

The Dataset Router is responsible for selecting the appropriate Builder.

It never performs semantic analysis.

Routing decisions are based exclusively on Dataset Descriptors.

The Router must remain deterministic.

Knowledge origin, provider or acquisition method must never influence routing decisions.

---

# Builder Framework

Builders transform Repository objects into training datasets.

Every Builder follows the same lifecycle:

```text
prepare()

↓

normalize()

↓

build()

↓

validate()

↓

judge()

↓

publish()
```

Each Builder produces a different type of training dataset.

---

# Available Builders

## Corpus Builder

Produces language modeling datasets for foundation model pretraining.

Output:

* train.txt
* validation.txt

---

## Knowledge Builder

Transforms Repository knowledge into structured knowledge objects.

These objects may later support retrieval, evaluation or future learning tasks.

---

## Conversation Builder

Produces instruction-following and conversational datasets.

Primary consumer:

Supervised Fine-Tuning (SFT)

---

## Preference Builder

Produces preference datasets containing preferred and rejected responses.

Primary consumer:

Direct Preference Optimization (DPO)

---

# Judge Framework

Before publication, every Builder output passes through the Judge Framework.

The framework evaluates:

* structural validity
* knowledge consistency
* conversation quality
* instruction quality
* preference quality
* safety
* overall confidence

Only validated datasets become eligible for training.

---

# Training Platform

The Training Platform consumes published datasets and trains models.

Three independent learning stages are defined.

## Stage 1 — Pretraining

Objective:

Learn language.

Produces:

Foundation models.

---

## Stage 2 — Supervised Fine-Tuning (SFT)

Objective:

Learn behavior.

Consumes:

Conversation datasets.

Produces:

Instruction-following assistants.

---

## Stage 3 — Direct Preference Optimization (DPO)

Objective:

Learn preference.

Consumes:

Preference datasets.

Produces:

Higher-quality behavioral responses.

---

# Separation of Learning

The three learning stages are independent.

Pretraining does not replace SFT.

SFT does not replace DPO.

Each stage teaches a different capability.

Sharing a common Repository does not imply sharing identical datasets.

---

# Future Extensions

The architecture is intentionally designed for future expansion.

Possible future Builders include:

* Vision Builder
* Speech Builder
* Video Builder
* Code Builder
* Agent Builder
* Simulation Builder

No architectural redesign should be required to support these capabilities.

---

# Summary

The Learning Architecture establishes a modular pipeline that separates knowledge acquisition, dataset generation, validation and model training.

Every learning stage has a clearly defined purpose, communicates through stable contracts and relies on a shared Repository as its common knowledge foundation.

Subsequent blueprint documents define each subsystem in detail.
