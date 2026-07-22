# Domain Model: Knowledge Entities

> Companion to `glossary/README.md`. See that document for the canonical concise definitions.

## Learning Node

**Definition**: The atomic, addressable unit of knowledge in the Knowledge Graph. Written as
"Learning Node" in prose (`glossary/README.md`) and as `LearningNode` in code
(`skills/python.md`'s naming convention) — the same entity, two representations, not two concepts.

| Attribute | Type | Notes |
|---|---|---|
| `id` | identifier | |
| `label`, `description` | string | |
| `status` | enum | Candidate → Reviewed → Published → Deprecated (`specs/knowledge-engine.md`) |
| `provenance` | `{document_id, topic_ids}` | Topic-level only — never Chunk-level (ADR-002) |

**Relationships**: connected to other Learning Nodes via Knowledge Edges; targeted by
`GenerationTask`s (`docs/domain-model/generation-entities.md`); has a per-Student `Confidence`
(`docs/domain-model/learning-state-entities.md`).

**Business rule**: "A Validated Learning Node Never Becomes Invalid" (`docs/domain-rules/README.md`)
— once a Learning Node passes structural review, it may be *deprecated* by a later re-extraction,
but its historical validity is never retroactively revoked.

## Knowledge Edge

**Definition**: A typed, directed relationship between two Learning Nodes.

| Attribute | Type | Notes |
|---|---|---|
| `from_node_id`, `to_node_id` | reference | |
| `edge_type` | enum | At minimum `prerequisite_of` (`specs/knowledge-engine.md`) |

**Relationships**: connects exactly two Learning Nodes. The full graph of Learning Nodes + Knowledge
Edges is the Knowledge Graph.

## Knowledge Graph

**Definition**: The full directed graph of Learning Nodes and Knowledge Edges, representing
everything the platform currently understands is possible to know for a given body of Documents.

**Not personalized** — see `docs/domain/knowledge-engine.md`'s "map vs. route" distinction. A
Student's personalized traversal of it is a **Learning Path**, defined next.

## Learning Path

**Definition**: A Student-specific, dynamically computed sequence or subset of Learning Nodes
representing the platform's current recommendation for what this Student should engage with next
and in what order — a *derived view* over the Knowledge Graph and the Student's Learning State, not
a separately stored or hand-authored curriculum sequence.

| Attribute | Type | Notes |
|---|---|---|
| `student_id` | reference | |
| `ordered_node_ids` | list of reference | Recomputed, not persisted as a fixed sequence |
| `computed_at` | timestamp | |

**Relationships**: computed from Knowledge Graph structure + Learning State, consumed by Generation
Engine's target-selection logic (`specs/generation-engine.md` R1) and by
`SessionPlan`/`ChapterPlan` (`docs/domain-model/generation-entities.md`), which turn a slice of the
Learning Path into concrete, scheduled content.

**Important distinction**: a Learning Path is *not* a new source of truth — like `Learning State`, it
is always recomputable from the Knowledge Graph and Learning State, following the same
derived-projection discipline ADR-004 established for Confidence. No component persists a Learning
Path as if it were authoritative on its own.

## Related Documents

`glossary/README.md`, `specs/knowledge-engine.md`, `docs/domain/knowledge-engine.md`,
`docs/pedagogy/chunking.md`, `docs/pedagogy/pareto-principle.md`.
