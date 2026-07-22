# Domain Model: Ingestion Entities

> Companion to `glossary/README.md`, which remains the canonical one-paragraph definition of each
> term (`.ai/constitution.md` Article VI). This document adds attributes, relationships, and
> examples the glossary deliberately keeps concise.

## Document

**Definition**: A raw source of educational material submitted for ingestion.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `title` | string | |
| `source_format` | enum | `pdf`, `text` (see `specs/document-ingestion.md` R1) |
| `status` | enum | Per `specs/document-ingestion.md`'s state diagram |

**Relationships**: has many Chapters. **Immutable once ingested** — "Paper Is Immutable"
(`docs/domain-rules/README.md`); a Document is never edited in place, only re-ingested as a new
Document if the source material changes.

## Chapter

**Definition**: A top-level structural division of a Document.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `document_id` | reference | |
| `order` | integer | |
| `title` | string | |

**Relationships**: belongs to one Document; has many Topics. A **ChapterPlan**
(`docs/domain-model/generation-entities.md`) is a separate, generated planning artifact describing
*how* a Chapter's Learning Nodes should be sequenced across Study Sessions — it is not part of the
Chapter entity itself.

## Topic

**Definition**: A coherent subject-matter unit within a Chapter — the granularity at which Learning
Nodes are extracted.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `chapter_id` | reference | |
| `order` | integer | |
| `title`, `summary` | string | |

**Relationships**: belongs to one Chapter; source of one or more Learning Nodes
(`docs/domain-model/knowledge-entities.md`), tracked via Topic-level provenance only (ADR-002).

## Chunk *(internal only — Ingestion Engine)*

**Definition**: The smallest unit of text Ingestion Engine produces for its own internal processing.

Never appears in a published contract, an API, or any Student-facing surface (ADR-002). Not to be
confused with **Chunking**, the cognitive-science principle (`docs/pedagogy/chunking.md`) — see
that document's disambiguation note.

## OCRResult *(internal only — Ingestion Engine)*

**Definition**: The text extracted from a scanned or image-based Document page by optical character
recognition, before it is split into Chunks.

**Rationale for internal-only status**: like Chunk, an OCRResult's structure is entirely a function
of the OCR technique used and carries no pedagogical meaning of its own — extending ADR-002's
reasoning by analogy. Ingestion Engine may need to correct or re-run OCR without that ever being
visible to any other Engine.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | Private to Ingestion Engine |
| `document_id` | reference | |
| `raw_text` | string | Pre-Chunk, pre-cleanup |
| `confidence_score` | float | OCR engine's own confidence, unrelated to Learning State's Confidence |

## Embedding *(internal only — Ingestion Engine)*

**Definition**: A vector representation of a Chunk or Topic, used internally for similarity-based
processing during extraction (e.g., feeding Knowledge Engine's extraction technique, or detecting
near-duplicate content across Documents per `automation/ai-automation/knowledge-base-validation.md`'s
cross-Document overlap check).

**Rationale for internal-only status**: an embedding's dimensionality and semantics are entirely a
function of the model that produced it — publishing it would hard-couple every consumer to that
specific model, directly contradicting Article I of `.ai/constitution.md`.

## Related Documents

`glossary/README.md`, `specs/document-ingestion.md`, ADR-002, `docs/domain/knowledge-engine.md`.
