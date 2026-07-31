# Dataset Router

**Version:** 1.0
**Status:** Draft (Frozen after approval)

---

# Purpose

The Dataset Router is responsible for directing Repository objects to the appropriate Builder based exclusively on their Dataset Descriptor.

The Router never analyzes Repository content.

The Router never trains models.

The Router never modifies Repository objects.

Its only responsibility is deterministic routing.

---

# Philosophy

The Router follows one fundamental rule:

> **Analyze once. Route many times.**

Repository objects are analyzed exactly once by the Dataset Intelligence Engine.

Every subsequent routing decision must reuse the existing Dataset Descriptor.

The Router must never duplicate analysis logic.

---

# Position in the Learning Pipeline

```text
Repository
      │
      ▼
Dataset Intelligence Engine
      │
      ▼
Dataset Descriptor
      │
      ▼
Dataset Router
      │
 ┌────┼────┬────┬────┐
 ▼    ▼    ▼    ▼
Corpus
Knowledge
Conversation
Preference
Builders
```

The Router sits between analysis and dataset generation.

---

# Responsibilities

The Dataset Router is responsible for:

* reading Dataset Descriptors
* selecting Builders
* dispatching Repository references
* supporting multiple Builder destinations
* logging routing decisions

The Router is **not** responsible for:

* semantic analysis
* quality evaluation
* document parsing
* model training

---

# Design Principles

## Deterministic

The same Descriptor must always produce the same routing result.

No randomness is allowed.

---

## Explainable

Every routing decision must be explainable.

Example:

```
Conversation Builder selected

Reason:

contains_dialogue = true

supports_conversation = true

quality_score = 92
```

---

## Source Independent

The Router must never inspect:

* acquisition source
* connector type
* website
* dataset provider

Only the Dataset Descriptor may influence routing.

---

## Stateless

The Router maintains no internal learning state.

Routing depends entirely on the Descriptor provided for the current Repository object.

---

# Routing Pipeline

```text
Dataset Descriptor

↓

Schema Validation

↓

Capability Validation

↓

Quality Validation

↓

Rule Evaluation

↓

Builder Selection

↓

Dispatch
```

---

# Stage 1 — Schema Validation

Ensure the Descriptor conforms to the current schema version.

If validation fails:

Reject routing.

Log the reason.

---

# Stage 2 — Capability Validation

Verify that required capability flags exist.

Examples:

supports_pretraining

supports_conversation

supports_preference

supports_knowledge

Missing capabilities prevent routing.

---

# Stage 3 — Quality Validation

Minimum quality thresholds may be configured.

Example:

```
quality_score >= 80
```

Objects below threshold may be:

* rejected
* reviewed
* routed only to selected Builders

Policy determines the outcome.

---

# Stage 4 — Rule Evaluation

The Router evaluates deterministic routing rules.

Example:

```
supports_pretraining == true

↓

Corpus Builder
```

```
supports_conversation == true

↓

Conversation Builder
```

```
supports_preference == true

↓

Preference Builder
```

A Repository object may satisfy multiple rules simultaneously.

---

# Stage 5 — Builder Selection

Routing is not exclusive.

One Repository object may be dispatched to:

* Corpus Builder
* Knowledge Builder

at the same time.

Likewise,

a dialogue dataset may be routed to:

* Conversation Builder

and later contribute to Preference Builder after additional processing.

---

# Stage 6 — Dispatch

The Router sends only:

* Repository Identifier
* Dataset Descriptor

Builders retrieve Repository content when necessary using the Repository identifier.

Builders never receive raw external files.

---

# Routing Rules

Routing rules must remain declarative.

They should not be embedded inside application logic.

Example:

```
IF

supports_conversation = true

AND

quality_score >= 85

THEN

Conversation Builder
```

Rules should be configurable without changing Router implementation.

---

# Multiple Destinations

Routing supports one-to-many relationships.

Example:

```
Repository Object

↓

Corpus Builder

Knowledge Builder
```

Another example:

```
Conversation Dataset

↓

Conversation Builder

↓

Preference Builder
```

The Router should support parallel dispatch whenever appropriate.

---

# Routing Report

Every routing operation generates a Routing Report.

Typical information includes:

* Repository ID
* Descriptor Version
* Matching Rules
* Selected Builders
* Rejected Builders
* Routing Time
* Policy Version

Reports provide complete traceability.

---

# Failure Handling

Possible routing outcomes:

Success

Review Required

Rejected

Retry

Failures never modify Repository objects.

---

# State Machine

```
Idle

↓

Receive Descriptor

↓

Validate

↓

Evaluate Rules

↓

Select Builders

↓

Dispatch

↓

Completed
```

Failure states:

* Validation Failed
* Policy Rejected
* Dispatch Failed

---

# Extensibility

Future Builders should require only:

* registering a Builder
* defining routing rules

The Router itself should remain unchanged.

---

# Relationship with Builders

The Router selects Builders.

Builders never influence routing decisions.

Builders consume Repository references and Dataset Descriptors only after routing has completed.

---

# Summary

The Dataset Router is the deterministic decision layer of the Learning Platform.

It converts analyzed Dataset Descriptors into Builder assignments using transparent, reproducible routing rules.

By separating analysis, routing and dataset generation, the Router ensures that ARYA remains modular, explainable and extensible as new learning capabilities are introduced.
