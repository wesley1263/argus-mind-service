# Prompt Design Guide: Generation Engine (Orchestration)

**Job**: the umbrella guide for how a `PromptContext` (`docs/domain-model/generation-entities.md`)
becomes a concrete model call, regardless of content type. Content-type-specific guides
(`quiz-generation-prompt.md`, `game-generation-prompt.md`, etc.) extend the shared contract defined
here and in `adaptive-content-prompt.md`.

## Required Context (PromptContext)

Always: target Learning Node (label, description), its real Knowledge Edges (for Elaboration),
current Confidence, requested content type, calibrated Difficulty, target Bloom's level
(`docs/pedagogy/bloom-taxonomy.md`). Never: Chunk data, another Student's data, raw database rows.

## Instructions Every Generation Prompt Must Encode

1. **Generation Effect default** (`docs/pedagogy/generation-effect.md`): ask the Student to produce
   before revealing an answer/explanation, unless the content type is explicitly introductory
   (Priming-stage, `docs/pedagogy/priming.md`).
2. **Desirable Difficulty** (`docs/pedagogy/desirable-difficulties.md`): calibrate to the supplied
   Difficulty value, which is already relative to this Student's Confidence — do not independently
   re-judge "how hard this should be."
3. **Cognitive load discipline** (`docs/pedagogy/cognitive-load-theory.md`): introduce a bounded
   number of new elements; no decorative or redundant content.
4. **Tone** (`docs/domain-rules/README.md` — "the child is never blamed"): never frame difficulty or
   struggle as a property of the Student.
5. **Grounding**: any factual claim must trace to the supplied Learning Node/Knowledge Edge data —
   never invented content presented as fact.

## Selection Logic Lives Outside the Prompt

Which Learning Node, which content type, and which Difficulty to request is decided by
deterministic application code (`specs/generation-engine.md` R1) *before* this prompt runs — the
prompt's job is producing content for an already-made decision, never re-deciding what to generate.

## What Validation Must Check

Every item in `prompt-review-checklist.md`, plus content-type-specific criteria from the relevant
specific guide.

## Related Documents

`specs/generation-engine.md`, `docs/domain/generation-engine.md`,
`docs/prompt-engineering/adaptive-content-prompt.md`,
`docs/prompt-engineering/prompt-review-checklist.md`, `skills/prompt-engineering.md`.
