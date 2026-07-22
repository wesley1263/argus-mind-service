# Domain Model: Evidence Entities

> Companion to `glossary/README.md`. See that document for the canonical concise definitions.

## Session (a.k.a. Study Session)

**Definition**: A bounded period of interaction between a Student and the platform. "Session" is the
engineering/data term (`glossary/README.md`, owned by Evidence Engine); "Study Session" is the same
entity referred to in product and pedagogy contexts (`docs/domain/study-session.md`,
`specs/study-session.md`). There is one entity, not two.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `student_id` | reference | |
| `started_at`, `ended_at` | timestamp | `ended_at` null while Active |

**Relationships**: has many Evidence records; a **SessionPlan**
(`docs/domain-model/generation-entities.md`) may exist as the intended content sequence for a
Session, generated before or at its start — the Plan is a separate, generated artifact, not an
attribute of the Session entity itself.

## Evidence

**Definition**: An immutable, timestamped observation of a Student's interaction or performance
with respect to one or more Learning Nodes.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `session_id`, `learning_node_id` | reference | |
| `evidence_type` | enum | See EvidenceType below |
| `observation` | structured data | Shape depends on `evidence_type` |
| `recorded_at` | timestamp | |
| `supersedes_id` | reference, nullable | Corrections are new records (ADR-003) |

**Business rule**: "Evidence Accumulates Forever" (`docs/domain-rules/README.md`) — never edited,
never deleted (ADR-003).

## EvidenceType

**Definition**: A classification of what kind of observation an Evidence record represents —
formalizes `specs/evidence-engine.md`'s `observation` field into an explicit, extensible taxonomy
rather than an unstructured blob.

| Value | What it captures |
|---|---|
| `answer_submitted` | A direct response to a Quiz or similar prompt. |
| `self_explanation` | A Feynman-technique-style free-text explanation (`docs/pedagogy/feynman-technique.md`). |
| `self_reported_confidence` | A Student's own prediction of their understanding (`docs/pedagogy/metacognition.md`) — never used as a direct Confidence input, only as a comparison point. |
| `hint_requested` | A signal of struggle independent of the eventual answer's correctness. |
| `time_on_task` | Duration spent before responding. |
| `mindmap_interaction` | A manipulation event on a MindMap (`docs/pedagogy/motor-encoding.md`). |
| `game_outcome` | Result of a Game interaction. |

**Why this matters**: Learning State's confidence model (`docs/domain/learning-state-engine.md`) can
weight different `EvidenceType`s differently — a `self_explanation` that correctly connects a
Learning Node to its prerequisite is stronger evidence of understanding than a lucky
`answer_submitted` on a multiple-choice Quiz (`docs/pedagogy/active-recall.md`'s recall-vs-recognition
distinction).

## Related Documents

`glossary/README.md`, `specs/evidence-engine.md`, ADR-003, `docs/domain/evidence-engine.md`,
`docs/pedagogy/active-recall.md`, `docs/pedagogy/metacognition.md`.
