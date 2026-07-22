# Prompt Design Guide: Quiz Generation

**Job**: generate a Quiz — the primary vehicle for Retrieval Practice and Active Recall
(`docs/pedagogy/retrieval-practice.md`, `docs/pedagogy/active-recall.md`).

## Required Context

Standard `PromptContext`, plus the explicit target Bloom's level
(`docs/pedagogy/bloom-taxonomy.md`) — a Quiz prompt without a stated cognitive-level target defaults
toward the easiest-to-generate Remember-level recall question, which this guide exists specifically
to prevent.

## Instructions the Prompt Must Encode

1. **Default to recall/production over recognition** (`docs/pedagogy/active-recall.md`) — open
   response preferred over multiple-choice wherever the content and target Bloom's level allow it.
2. **Match the requested Bloom's level structurally**: an Apply-level question must require applying
   the concept to a new situation not verbatim present in the Learning Node's description, not just
   restate the definition with different wording.
3. **One concept chunk per question** (`docs/pedagogy/chunking.md`, `docs/pedagogy/cognitive-load-theory.md`)
   — a question testing two unrelated facts at once conflates the resulting Evidence's meaning.
4. **Distractors (if multiple-choice) must be plausible, not arbitrary** — a distractor that's
   obviously wrong provides no discriminative Evidence about understanding; distractors should
   represent common, specific misconceptions relevant to the Learning Node where possible.

## What Validation Must Check

Bloom's-level structural match; grounding in real Learning Node content; that a stated correct
answer is actually correct relative to the Knowledge Graph (not just internally consistent with the
question's own framing).

## Related Documents

`docs/pedagogy/retrieval-practice.md`, `docs/pedagogy/active-recall.md`,
`docs/pedagogy/bloom-taxonomy.md`, `docs/prompt-engineering/adaptive-content-prompt.md`.
