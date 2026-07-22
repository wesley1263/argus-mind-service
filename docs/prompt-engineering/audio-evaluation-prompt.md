# Prompt Design Guide: Audio Evaluation

> **Status note**: full voice interaction is a `docs/future/README.md` capability (Voice Tutor).
> This guide covers the narrower, nearer-term case of a Student's *spoken* self-explanation,
> transcribed and then evaluated — not real-time conversational tutoring.

**Job**: evaluate a transcribed spoken self-explanation. Builds directly on
`summary-evaluation-prompt.md`'s completeness/correctness checks, with transcription-specific
handling added.

## Required Context

The audio transcript (already produced by a speech-to-text step — transcription itself is not this
prompt's job); the same Learning Node context as `summary-evaluation-prompt.md`.

## What's Different From Text Evaluation

1. **Transcription artifacts**: filler words, false starts, and self-corrections are common in
   spoken explanation and are not evidence of misunderstanding — the prompt must be instructed to
   evaluate the *final, corrected* meaning of a self-correcting statement, not penalize the initial
   misstatement within it.
2. **No claim about prosody or confidence-from-tone**: this platform does not evaluate *how*
   something was said (hesitation, tone) as a proxy for understanding — that would be an
   unvalidated, speculative signal. Only the transcribed content is evaluated, using exactly
   `summary-evaluation-prompt.md`'s completeness/correctness/jargon checks.

## Checks This Prompt Must Perform

Identical to `summary-evaluation-prompt.md`, applied to the corrected transcript — this document
does not introduce new evaluation criteria, only the transcription-handling instruction above.

## Why This Is Marked Exploratory

Motor Encoding (`docs/pedagogy/motor-encoding.md`) already notes the platform is cautious about
overclaiming benefits from interaction modality. The same caution applies here: speaking an
explanation may or may not add pedagogical value beyond typing it, and this guide takes no position
on that until real evaluation data exists (`docs/evaluation/README.md`).

## Related Documents

`docs/prompt-engineering/summary-evaluation-prompt.md`, `docs/pedagogy/motor-encoding.md`,
`docs/future/README.md` (Voice Tutor).
