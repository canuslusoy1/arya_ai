# Builder Framework

**Version:** 1.0

**Status:** Draft (Frozen after approval)

---

# Purpose

The Builder Framework transforms Repository knowledge into high-quality training datasets.

Builders are not responsible for deciding **whether** knowledge should be used.

They are responsible for transforming already approved Repository objects into datasets suitable for specific learning objectives.

Every Builder follows the same lifecycle and contract.

---

# Philosophy

Builders implement one principle:

> Transform, never interpret.

Analysis belongs to the Dataset Intelligence Engine.

Routing belongs to the Dataset Router.

Validation belongs to the Judge Framework.

Training belongs to the Training Platform.

Builders only perform transformation.

---

# Builder Lifecycle

Every Builder follows the same lifecycle.

```
Repository Object

↓

Load

↓

Candidate Generation

↓

Normalization

↓

Internal Validation

↓

Judge

↓

Publication

↓

Training Platform
```

No Builder may bypass any stage.

---

# Stage 1 — Load

Input:

* Repository ID
* Dataset Descriptor

The Builder loads the Repository object.

Builders never access external sources.

---

# Stage 2 — Candidate Generation

The Builder generates one or more candidate training examples.

Examples:

Corpus Builder

↓

paragraphs

Conversation Builder

↓

instruction / response pairs

Preference Builder

↓

chosen / rejected pairs

Knowledge Builder

↓

facts

Candidates are temporary objects.

They are not yet training data.

---

# Stage 3 — Normalization

Builders normalize generated candidates.

Examples:

* whitespace cleanup
* formatting
* JSON schema
* metadata alignment
* token normalization

Normalization should never alter meaning.

---

# Stage 4 — Internal Validation

Builders perform lightweight validation.

Examples:

* empty response
* malformed JSON
* missing fields
* excessive length
* duplicated candidates

Only structurally valid candidates continue.

---

# Stage 5 — Judge Framework

Candidates are submitted to Judges.

Builders never decide publication.

Judges assign:

* quality
* confidence
* rejection reason

Builders simply receive the verdict.

---

# Stage 6 — Publication

Approved candidates become official datasets.

Rejected candidates remain archived.

Nothing is silently discarded.

---

# Builder Contract

Every Builder implements:

```
prepare()

load()

generate()

normalize()

validate()

publish()
```

Optional hooks:

```
before_generate()

after_generate()

before_publish()

after_publish()
```

---

# Builder Independence

Builders must remain independent.

No Builder may directly invoke another Builder.

Instead:

Publication events are emitted.

Other Builders may subscribe.

This keeps the architecture loosely coupled.

---

# Parallel Execution

Builders are independent workers.

Multiple Builders may process the same Repository object simultaneously.

Example:

```
Repository

↓

Corpus Builder

Knowledge Builder
```

Both execute independently.

---

# Builder Outputs

Each Builder produces one dataset type.

Corpus Builder

↓

train.txt

Conversation Builder

↓

instruction dataset

Preference Builder

↓

chosen / rejected pairs

Knowledge Builder

↓

structured facts

Builders never train models.

---

# Metadata Preservation

Every generated sample preserves provenance.

Each sample records:

* Repository ID
* Builder Version
* Generation Timestamp
* Descriptor Version
* Judge Version

Every training example remains traceable.

---

# Extensibility

Adding a new Builder requires only:

* implementing the Builder contract
* registering Builder metadata
* defining Router rules

Existing Builders remain unchanged.

---

# Failure Handling

Builder failures never affect Repository integrity.

Possible outcomes:

Success

Rejected

Retry

Paused

Failed

Partial publication is supported.

---

# State Machine

```
Idle

↓

Loading

↓

Generating

↓

Normalizing

↓

Validating

↓

Judging

↓

Publishing

↓

Completed
```

---

# Summary

The Builder Framework provides a unified transformation pipeline between Repository knowledge and training datasets.

By separating transformation from analysis, routing, validation and training, Builders remain modular, reusable and easy to extend.

Every future Builder—including multimodal Builders—must follow the same lifecycle and contract.
