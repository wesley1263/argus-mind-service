# Domain Model: Learning State Entities

> Companion to `glossary/README.md`. See that document for the canonical concise definitions.

## Learning State

**Definition**: A Student's current, derived understanding of their own mastery, expressed per
Learning Node.

| Attribute | Type | Notes |
|---|---|---|
| `student_id`, `learning_node_id` | reference | Composite identity |
| `confidence` | float | See Confidence below |
| `model_version` | reference | `docs/domain/learning-state-engine.md` |
| `computed_at` | timestamp | |

**Business rule**: "Learning State Is Estimated" (`docs/domain-rules/README.md`) — always a
projection over Evidence (ADR-004), never a value anyone writes directly. Never persisted as if it
were a fact independent of the Evidence it was computed from.

## Confidence

**Definition**: A normalized, per-(Student, Learning Node) score representing the platform's
estimated probability that the Student has mastered that Learning Node.

| Property | Value |
|---|---|
| Range | `[0.0, 1.0]`, continuous — never a boolean mastered/not-mastered flag |
| Decay | Decreases over time without new relevant Evidence (`docs/pedagogy/spaced-repetition.md`) |
| Write path | None outside Learning State Engine's own computation (ADR-004) |

**Business rule**: "Confidence Is Never Binary" (`docs/domain-rules/README.md`) — reflects genuine
uncertainty rather than forcing a premature mastered/not-mastered judgment. "Mastery" is the
informal way to talk about a *high* Confidence value in prose (`glossary/README.md`) — it is never a
separate stored field.

**Asymmetric error cost**: per `docs/domain-rules/README.md` and `docs/domain/learning-state-engine.md`,
a **false negative** (Confidence too low for a Learning Node the Student actually understands) is
treated as worse than a **false positive** (Confidence too high, briefly) — this asymmetry should
inform how the confidence model is tuned, not just how it's described.

## Related Documents

`glossary/README.md`, `specs/learning-state-engine.md`, ADR-004,
`docs/domain/learning-state-engine.md`, `docs/pedagogy/spaced-repetition.md`,
`docs/pedagogy/metacognition.md`.
