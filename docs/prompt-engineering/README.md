# Prompt Engineering

Prompt design guides for every generative surface in the platform, plus the cross-cutting review,
versioning, regression, and evaluation strategies that govern all of them. Complements
`skills/prompt-engineering.md` (engineering conventions) and `testing/prompt-evaluation.md` /
`automation/ai-automation/` (the CI mechanics) — this folder is where the actual pedagogical content
of each prompt is specified.

## Per-Engine / Per-Purpose Guides

| Guide | Produces |
|---|---|
| [`knowledge-engine-prompt.md`](knowledge-engine-prompt.md) | Candidate Learning Nodes and Knowledge Edges from a Topic. |
| [`evidence-engine-prompt.md`](evidence-engine-prompt.md) | Structured `EvidenceType`/observation from raw Student input — never a judgment. |
| [`learning-state-prompt.md`](learning-state-prompt.md) | Structured per-claim judgment feeding the deterministic confidence model — never Confidence itself. |
| [`generation-engine-prompt.md`](generation-engine-prompt.md) | The shared orchestration contract every content-type prompt follows. |

## Content-Type Guides (extend `adaptive-content-prompt.md`)

| Guide | Content type |
|---|---|
| [`adaptive-content-prompt.md`](adaptive-content-prompt.md) | Shared base contract. |
| [`mindmap-validation-prompt.md`](mindmap-validation-prompt.md) | MindMap |
| [`summary-evaluation-prompt.md`](summary-evaluation-prompt.md) | Summary / Feynman-technique self-explanation |
| [`audio-evaluation-prompt.md`](audio-evaluation-prompt.md) | Spoken self-explanation (transcribed) |
| [`game-generation-prompt.md`](game-generation-prompt.md) | Game |
| [`quiz-generation-prompt.md`](quiz-generation-prompt.md) | Quiz |

## Cross-Cutting Strategy

| Document | Governs |
|---|---|
| [`prompt-review-checklist.md`](prompt-review-checklist.md) | What every prompt above is checked against before shipping. |
| [`prompt-versioning-strategy.md`](prompt-versioning-strategy.md) | What a version identifies, when it changes, how it's tracked. |
| [`prompt-regression-strategy.md`](prompt-regression-strategy.md) | What counts as a regression, and triage priority when one occurs. |
| [`prompt-evaluation-metrics.md`](prompt-evaluation-metrics.md) | The concrete rubric behind each evaluation dimension, per content type. |

## The One Rule Every Guide in This Folder Shares

No prompt in this folder ever directly sets Confidence, Learning State, or bypasses Validation
(ADR-004, ADR-005) — every prompt either produces Student-facing content that Validation gates, or
produces a structured signal that deterministic code interprets. This split is what lets the
platform use generative techniques throughout its pipeline while keeping the Constitutional
integrity guarantees (`.ai/constitution.md` Article IV, V) intact.

## Related Documents

`skills/prompt-engineering.md`, `testing/prompt-evaluation.md`, `automation/ai-automation/`,
`docs/domain-model/generation-entities.md`.
