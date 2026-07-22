# Prompt Regression Strategy

Expands `automation/ai-automation/prompt-regression.md`'s CI mechanics with the pedagogical
reasoning behind what counts as a regression and how it should be triaged.

## Two Sources of Regression

1. **We changed the prompt** (a new version, per `prompt-versioning-strategy.md`) — caught at
   change time by `automation/ai-automation/prompt-evaluation-workflow.md`.
2. **The model provider changed underneath us** with no prompt-side change at all — caught by the
   nightly `automation/ai-automation/prompt-regression.md` run against the still-active version.

Both are regressions in the sense that matters to a Student; only the first is something engineering
directly caused, which changes who's responsible for the fix but not the urgency of noticing it.

## What Counts as a Regression, Pedagogically

Not just "the score went down." A regression in the **false-negative direction** on Learning State
judgments (`docs/domain-rules/README.md`'s asymmetry) is more urgent than an equivalent drop in a
less safety-relevant dimension like tone polish — `prompt-evaluation-metrics.md`'s
dimension-by-dimension reporting exists specifically so this kind of triage is possible instead of
hidden inside one composite number.

## Triage Priority

1. Any regression in Correctness or Safety dimensions on Generation Engine content — treat as a
   Constitutional-severity issue (ADR-005) regardless of magnitude.
2. Any regression suggesting increased false negatives in Learning State claim-judgment scoring
   (`docs/prompt-engineering/learning-state-prompt.md`) — per the asymmetry rule.
3. Pedagogical-soundness regressions (wrong Bloom's level, lost Elaboration grounding) — high
   priority but not an emergency halt.
4. Tone/style drift — lowest priority, tracked but rarely blocking on its own.

## Why Regression Testing Can't Stop at "Does It Still Pass"

A prompt can keep passing every golden-dataset scenario while still drifting in a direction that
matters — e.g., systematically producing correct but increasingly bland, low-Elaboration content.
`prompt-evaluation-metrics.md`'s dimensions exist to make this kind of slow, still-passing drift
visible as a trend, not just catch hard failures.

## Related Documents

`automation/ai-automation/prompt-regression.md`, `docs/prompt-engineering/prompt-evaluation-metrics.md`,
`docs/domain-rules/README.md`, `docs/prompt-engineering/prompt-versioning-strategy.md`.
