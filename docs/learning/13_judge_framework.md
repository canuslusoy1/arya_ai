# Judge Framework

**Version:** 1.0

**Status:** Draft (Frozen after approval)

---

# Purpose

The Judge Framework is the independent quality assurance layer of the Dataset Platform.

Its responsibility is to evaluate Builder outputs before they become official training datasets.

Judges never modify data.

Judges never generate data.

Judges only evaluate and produce verdicts.

Every dataset must pass through the Judge Framework before publication.

---

# Design Philosophy

The Judge Framework follows one principle:

> **No training without validation.**

Every candidate dataset must satisfy objective quality requirements before it becomes eligible for model training.

Builders produce.

Judges evaluate.

The Dataset Registry publishes.

Training Platform consumes.

Each responsibility remains independent.

---

# Position in the Dataset Platform

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

The Judge Framework is responsible for:

* validating Builder outputs
* assigning quality scores
* assigning confidence scores
* detecting structural problems
* detecting semantic inconsistencies
* generating evaluation reports
* approving or rejecting publication

The Judge Framework is **not** responsible for:

* routing
* dataset generation
* repository management
* model training

---

# Judge Pipeline

Every candidate dataset follows the same evaluation pipeline.

```text
Candidate Dataset

↓

Schema Judge

↓

Integrity Judge

↓

Quality Judge

↓

Safety Judge

↓

Consistency Judge

↓

Score Aggregation

↓

Verdict
```

Each Judge contributes independently.

---

# Schema Judge

Purpose:

Verify structural correctness.

Typical checks:

* valid JSON
* required fields
* supported schema version
* field types
* missing values

Failure prevents further evaluation.

---

# Integrity Judge

Purpose:

Verify internal consistency.

Typical checks:

* duplicate samples
* corrupted records
* invalid references
* empty responses
* malformed conversations

---

# Quality Judge

Purpose:

Estimate dataset usefulness.

Typical signals:

* diversity
* completeness
* clarity
* redundancy
* information density

Produces:

* quality_score
* confidence

---

# Safety Judge

Purpose:

Detect undesirable content.

Examples:

* harmful instructions
* malformed preference pairs
* toxic samples
* prompt injection artifacts
* corrupted outputs

The Safety Judge protects downstream training.

---

# Consistency Judge

Purpose:

Verify semantic coherence.

Examples:

* instruction matches response
* chosen is better than rejected
* conversation flow is logical
* extracted facts match source

This Judge may use deterministic rules, statistical methods or optional LLM assistance.

---

# Verdict Aggregation

Individual Judges produce independent results.

The Aggregator combines them into a single decision.

Possible outcomes:

Approved

Approved with Warnings

Review Required

Rejected

The aggregation policy must remain configurable.

---

# Evaluation Report

Every evaluated dataset receives a permanent report.

Example fields:

* Dataset ID
* Builder Version
* Judge Versions
* Overall Score
* Confidence
* Warnings
* Failure Reasons
* Evaluation Timestamp

Reports are stored alongside the dataset.

---

# Quality Scores

Typical scoring range:

```text
95–100   Excellent
85–94    Very Good
70–84    Acceptable
50–69    Review Required
0–49     Rejected
```

Thresholds are configurable.

---

# Human Review

Some datasets cannot be judged with sufficient confidence.

These datasets enter a manual review queue.

Possible actions:

* approve
* reject
* request Builder regeneration

Manual decisions remain fully traceable.

---

# LLM Assistance

Local or cloud LLMs may assist specific Judges.

Possible use cases:

* semantic consistency
* conversation quality
* preference validation
* instruction quality

LLMs never produce the final verdict directly.

Their outputs become evidence for the Aggregator.

---

# State Machine

```text
Waiting

↓

Evaluating

↓

Aggregating

↓

Approved
        │
        ├── Published
        │
        └── Archived

Rejected

Review Required
```

---

# Extensibility

New Judges may be added without modifying existing ones.

Examples:

* Reasoning Judge
* Bias Judge
* Domain Judge
* Curriculum Judge
* Code Judge
* Vision Judge

Every Judge implements the common Judge contract.

---

# Interaction with Dataset Registry

Only datasets with an **Approved** verdict may be published.

The Dataset Registry never evaluates quality.

It trusts the Judge Framework.

---

# Interaction with Training Platform

The Training Platform never consumes candidate datasets.

It only consumes published datasets that have successfully passed the Judge Framework.

---

# Summary

The Judge Framework is the quality gate of the Dataset Platform.

By separating dataset generation from dataset validation, ARYA guarantees that every published dataset is traceable, reproducible and suitable for its intended learning objective.

The Judge Framework enables scalable quality assurance while remaining independent of Builders, the Dataset Registry and the Training Platform.
