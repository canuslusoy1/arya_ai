# Dataset Registry

**Version:** 1.0

**Status:** Draft (Frozen after approval)

---

# Purpose

The Dataset Registry is the authoritative catalog of every published training dataset within ARYA AI.

Builders generate candidate datasets.

Judges validate them.

Only datasets approved by the Judge Framework become Registry objects.

The Training Platform consumes datasets exclusively from the Dataset Registry.

---

# Design Philosophy

One principle governs the Dataset Registry.

> **Train only from published datasets.**

Training pipelines must never consume:

* Repository objects
* Builder outputs
* Candidate datasets

Only datasets published through the Registry are eligible for training.

---

# Position in the Learning Platform

```text
Repository
        │
        ▼
Dataset Intelligence Engine
        │
        ▼
Dataset Router
        │
        ▼
Builder Framework
        │
        ▼
Candidate Dataset
        │
        ▼
Judge Framework
        │
        ▼
Dataset Registry
        │
        ▼
Training Platform
```

---

# Responsibilities

The Dataset Registry is responsible for:

* registering published datasets
* versioning datasets
* preserving provenance
* storing Builder metadata
* storing Judge metadata
* tracking publication history
* exposing datasets to the Training Platform

It is not responsible for:

* building datasets
* judging datasets
* training models

---

# Dataset Object

Every published dataset becomes a Registry Object.

A Registry Object contains:

Identity

* Dataset ID
* Version
* Builder

Quality

* Judge score
* Confidence
* Validation report

Training

* Supported stages
* Curriculum level
* Recommended model size

Metadata

* Creation time
* Publication time
* Source Repository IDs
* Descriptor version

---

# Supported Training Stages

A single dataset may support multiple learning stages.

Examples:

Pretraining

Conversation SFT

Preference Optimization

Knowledge Extraction

Future training methods

The Registry stores compatibility explicitly.

---

# Versioning

Datasets are immutable.

Any modification creates a new version.

Example:

Dataset

↓

v1

↓

v2

↓

v3

Older versions remain reproducible.

---

# Provenance

Every dataset maintains complete lineage.

Training samples can always be traced back to:

Repository Object

↓

Builder Version

↓

Judge Version

↓

Publication

↓

Training Run

Nothing becomes anonymous.

---

# Dataset States

Every dataset has exactly one lifecycle state.

Draft

↓

Candidate

↓

Approved

↓

Published

↓

Archived

Only Published datasets are visible to the Training Platform.

---

# Publication Rules

Publication requires:

✓ successful Builder execution

✓ Judge approval

✓ Registry validation

Datasets failing publication remain archived.

They are never silently deleted.

---

# Registry Queries

The Registry supports deterministic queries.

Examples:

All datasets for SFT

All datasets produced by Conversation Builder

Datasets approved after a specific date

Datasets supporting curriculum level C2

Datasets generated from legal documents

The Registry never performs semantic search.

---

# Interaction with Training Platform

The Training Platform requests datasets.

The Registry decides which dataset versions are returned.

Training never accesses Builders directly.

This guarantees reproducibility.

---

# Extensibility

Future dataset categories require only:

* Builder support
* Judge support
* Registry metadata

No Registry redesign should be necessary.

---

# Summary

The Dataset Registry separates dataset generation from model training.

It guarantees that every training run consumes only validated, versioned and fully traceable datasets.

This architecture ensures reproducibility, transparency and long-term maintainability across all future learning stages.
