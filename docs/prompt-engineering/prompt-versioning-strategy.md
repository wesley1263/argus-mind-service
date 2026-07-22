# Prompt Versioning Strategy

Expands `skills/prompt-engineering.md`'s versioning requirement into a full strategy: what a version
number means, when it changes, and how it's used across the system.

## What a Prompt Version Identifies

A specific, immutable combination of: the prompt template text, the model/vendor configuration it's
paired with, and any structured examples embedded in the prompt (few-shot examples drawn from
`docs/golden-dataset/structure.md`). Changing *any* of these three produces a new version — a prompt
whose wording is unchanged but whose underlying model was upgraded is still a new version, because
its behavior can change.

## Version Identifier Format

`<engine>.<content-type-or-purpose>.v<N>` — e.g., `generation.quiz.v3`,
`learning-state.claim-judgment.v1`. This makes it immediately legible which of the fourteen prompt
guides in this folder a given version corresponds to, without a lookup table.

## What Every GenerationTask/Evidence/LearningState Record Stores

The exact prompt version that produced it (`docs/domain-model/generation-entities.md`,
`docs/domain-model/evidence-entities.md`) — never just "the current version," since "current" is
meaningless once it changes.

## When a New Version Is Required

- Any wording change to the prompt template.
- Any change to the underlying model or vendor.
- Any change to which few-shot examples are embedded.

A version is **not** required for: a change to the deterministic code that constructs
`PromptContext` (that's a code change, versioned normally per `automation/cicd/versioning-and-tagging.md`,
not a prompt version) — this keeps the two versioning schemes from being conflated.

## How a New Version Reaches Production

Follows `automation/ai-automation/prompt-evaluation-workflow.md` exactly: scored against
`docs/golden-dataset/structure.md`'s relevant dataset, compared dimension-by-dimension
(`docs/prompt-engineering/prompt-evaluation-metrics.md`) against the currently-active version, and
only activated if no dimension regresses past threshold.

## Deprecation, Not Deletion

An old prompt version is never deleted from the version history — records that cite it remain
explainable indefinitely (mirroring Evidence's own immutability principle, ADR-003, applied to
prompts). It's simply no longer the version new `GenerationTask`s are created with.

## Related Documents

`skills/prompt-engineering.md`, `automation/ai-automation/prompt-evaluation-workflow.md`,
`docs/prompt-engineering/prompt-regression-strategy.md`.
