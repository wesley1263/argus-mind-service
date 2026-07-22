# Prompt Design Guide: Evidence Engine

**Job**: structure raw Student input (free-text answers, spoken transcripts, self-explanations) into
a well-typed `EvidenceType` and `observation` payload **at capture time** — before persistence, not
after. This is the only place in the platform where a prompt runs on the write path into an
immutable log (ADR-003), so it must never *interpret* correctness — only *structure* what the
Student said.

## Required Context (PromptContext)

- The raw Student input (text or transcript).
- The Learning Node(s) the input is nominally about, for disambiguation only — not for judging
  correctness.

## Instructions the Prompt Must Encode

1. Classify the input into an `EvidenceType` (`docs/domain-model/evidence-entities.md`) based on
   its *form* (a direct answer vs. an explanation vs. a hint request), not its correctness.
2. Extract a structured `observation` payload matching that type's expected shape — e.g., for
   `self_explanation`, segment the free text into the claims it makes, without judging whether those
   claims are true.
3. **Never** attach a correctness verdict, a Confidence value, or any judgment of quality to the
   Evidence record this prompt helps produce — that would violate ADR-004 by smuggling a
   Learning-State-level judgment into the Evidence-capture step. Judgment happens later, in Learning
   State Engine, using `learning-state-prompt.md`.

## What Validation Must Check

That the structured output is a faithful, non-lossy structuring of the raw input — not a
summarization that could drop information relevant to a later, more careful judgment.

## Why This Prompt Is Higher-Risk Than Others

Because its output becomes a permanent, immutable record (ADR-003), a structuring error here can
never be corrected in place — only superseded by new Evidence. This prompt's Validation bar is
correspondingly stricter than a Generation Engine content prompt, where a bad output can simply be
regenerated.

## Related Documents

`specs/evidence-engine.md`, `docs/domain/evidence-engine.md`, ADR-003,
`docs/domain-model/evidence-entities.md`, `docs/prompt-engineering/learning-state-prompt.md`.
