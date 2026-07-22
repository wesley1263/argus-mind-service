# Domain Model: Generation Entities

> Companion to `glossary/README.md`. This is the largest entity group — Generation Engine's content
> types, planning artifacts, and supporting concepts. See `docs/domain/generation-engine.md` for the
> pedagogical rationale behind why these specific types exist.

## GenerationTask

**Definition**: A unit of work describing what learning content should be produced next.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `learning_node_id` | reference | Target node |
| `difficulty` | reference | See Difficulty below |
| `content_type` | enum | `MindMap`, `Summary`, `Game`, `Quiz`, `Reinforcement` |
| `bloom_level` | enum | `docs/pedagogy/bloom-taxonomy.md` |
| `status` | enum | Per `specs/generation-engine.md`'s state diagram |

## Validation

**Definition**: The mandatory gate every GenerationTask's output passes through before delivery
(ADR-005). Its outcome is `passed` or `failed(reason)`, recorded per attempt
(`specs/generation-engine.md`).

## Difficulty

**Definition**: A value object expressing how demanding a piece of content is, relative to a
Student's current Confidence for the target Learning Node — not an absolute scale independent of the
learner (`docs/pedagogy/desirable-difficulties.md`).

| Attribute | Type | Notes |
|---|---|---|
| `level` | float or enum | Calibrated relative to Confidence, not fixed |

## PromptContext

**Definition**: The structured, typed input assembled for a Generation Engine prompt — the target
Learning Node's label/description, its real Knowledge Edges, the Student's Confidence, and the
requested `content_type`/`Difficulty`/`bloom_level`. Formalizes
`skills/prompt-engineering.md`'s "Structure Every Prompt Around the Domain Contract" rule into an
explicit entity.

| Attribute | Type | Notes |
|---|---|---|
| `learning_node` | domain value | Never raw Chunk data (ADR-002) |
| `related_nodes` | list | Real Knowledge Edges, for Elaboration (`docs/pedagogy/elaboration.md`) |
| `student_confidence` | float | |
| `requested_content_type`, `difficulty`, `bloom_level` | — | |

**Why this is its own entity**: making PromptContext explicit and typed is what prevents an
adapter from string-interpolating raw database rows into a prompt — see
`skills/prompt-engineering.md`.

## Feedback

**Definition**: The response shown to a Student after they engage with content — distinct from
Evidence (the *observation* captured) and from Validation (the *pre-delivery* gate). Feedback is
Student-facing and reactive to a specific submission.

| Attribute | Type | Notes |
|---|---|---|
| `evidence_id` | reference | What this Feedback responds to |
| `content` | string | Should localize *what* was missing (`docs/pedagogy/feynman-technique.md`), not just right/wrong |

**Business rule**: "The Child Is Never Blamed" (`docs/domain-rules/README.md`) constrains Feedback
tone explicitly.

## Reinforcement

**Definition**: A GenerationTask specifically targeted at a low-Confidence Learning Node, generated
in response to struggle signals rather than as part of ordinary forward progression through a
Learning Path. The mechanism `docs/domain-rules/README.md`'s "Only Problematic Learning Nodes Are
Regenerated" rule refers to.

## AdaptiveContent

**Definition**: The umbrella, Student-facing term for anything Generation Engine produces —
MindMap, Summary, Game, Quiz, and Reinforcement are all *kinds of* AdaptiveContent. Used in product
copy and cross-content-type reporting (`docs/evaluation/README.md`) where the specific type doesn't
matter; engineering code should use the specific content type, not this umbrella term, once a
concrete type is known.

## The Content Types

### MindMap
Represents Knowledge Graph structure spatially (`docs/pedagogy/dual-coding.md`). Its structure must
correspond to real Knowledge Edges (`docs/pedagogy/dual-coding.md`'s Validation note).

### Summary
Prose covering a Learning Node or Topic. Most valuable when it requires Student generation
(`docs/pedagogy/generation-effect.md`), not pure passive reading.

### Game
A lightweight, engagement-optimized format (`docs/domain-principles.md` §10). May involve motor
interaction (`docs/pedagogy/motor-encoding.md`).

### Quiz
The primary vehicle for Retrieval Practice (`docs/pedagogy/retrieval-practice.md`) and Active Recall.

## SessionPlan

**Definition**: The ordered sequence of GenerationTasks intended for one Study Session, produced
before or at the Session's start, encoding the pacing/variety structure described in
`docs/domain/study-session.md` (opens easy, ramps into desirable difficulty, varies format).

| Attribute | Type | Notes |
|---|---|---|
| `session_id` | reference | |
| `planned_tasks` | ordered list of GenerationTask | Advisory — actual delivery adapts per response (`specs/study-session.md`) |

## ChapterPlan

**Definition**: A longer-horizon planning artifact describing how a Chapter's Learning Nodes should
be sequenced and paced across *multiple* future Study Sessions — the Chapter-level analog of a
SessionPlan, informed by Learning Path (`docs/domain-model/knowledge-entities.md`) and by
foundational-node prioritization (`docs/pedagogy/pareto-principle.md`).

| Attribute | Type | Notes |
|---|---|---|
| `chapter_id`, `student_id` | reference | Personalized, not a fixed curriculum |
| `planned_sequence` | ordered list of Learning Node references, grouped by intended Session | Advisory, recomputed as Learning State changes |

## Related Documents

`glossary/README.md`, `specs/generation-engine.md`, ADR-005, `docs/domain/generation-engine.md`,
`docs/prompt-engineering/README.md`, `docs/pedagogy/`.
