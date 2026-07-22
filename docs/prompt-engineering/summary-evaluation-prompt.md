# Prompt Design Guide: Summary Evaluation

**Job**: evaluate a Student-generated Summary or Feynman-technique self-explanation
(`docs/pedagogy/feynman-technique.md`) for conceptual completeness — this is the prompt behind
`evidence-engine-prompt.md`'s claim segmentation and `learning-state-prompt.md`'s per-claim judgment,
specifically applied to free-text explanatory content rather than short answers.

## Required Context

The Student's free-text Summary/explanation; the target Learning Node's real definition and
immediate Knowledge Edges; the specific parts of the concept a complete explanation should cover
(derived from the Learning Node's own structure, not a separately-authored rubric that could drift
from it).

## Checks This Prompt Must Perform

1. **Completeness**: which sub-parts of the concept are covered, hand-waved, or entirely missing —
   output per-part, not a single score, so Feedback (`feynman-technique.md`'s core value) can
   localize the gap specifically.
2. **Correctness**: does any covered part contradict the Learning Node's real definition?
3. **Jargon-without-substance detection**: does the explanation use correct terminology while
   avoiding the actual mechanism (the classic Feynman-Technique failure mode) — this check exists
   specifically because surface-level correct vocabulary can mask a real gap.

## Output

Structured per-part judgment (`covered` / `hand-waved` / `missing` / `incorrect`), which
`learning-state-prompt.md` consumes as one input to its own per-claim judgment, and which directly
drives Feedback content — never a single numeric grade.

## Related Documents

`docs/pedagogy/feynman-technique.md`, `docs/pedagogy/elaboration.md`,
`docs/prompt-engineering/learning-state-prompt.md`, `docs/domain-model/generation-entities.md`
(Feedback).
