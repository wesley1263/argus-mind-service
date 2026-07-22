# Prompt Review Checklist

Used to review any prompt in `docs/prompt-engineering/` — or a new one — before it ships. Every
item below has a specific document behind it; this checklist exists so review doesn't rely on
remembering all of them at once.

## Grounding

- [ ] Every factual claim the prompt could produce traces to real, supplied data (Learning Node,
      Knowledge Edges) — never the model's own general knowledge presented as this platform's
      content.
- [ ] No Chunk, OCRResult, or Embedding data appears anywhere in the prompt or its expected output
      (ADR-002 and its extensions, `docs/domain-model/ingestion-entities.md`).

## Pedagogy

- [ ] The prompt's default behavior matches the Generation Effect (produce before reveal) unless
      explicitly a Priming-stage or introductory exception (`docs/pedagogy/generation-effect.md`,
      `docs/pedagogy/priming.md`).
- [ ] Difficulty is respected as supplied, never independently softened
      (`docs/pedagogy/desirable-difficulties.md`).
- [ ] Content stays within a stated element/length bound appropriate to Cognitive Load Theory
      (`docs/pedagogy/cognitive-load-theory.md`).
- [ ] If the content type targets a specific Bloom's level, the prompt structurally requires that
      level, not just a differently-worded lower one (`docs/pedagogy/bloom-taxonomy.md`).

## Tone and Framing

- [ ] Feedback and evaluation prompts never describe the Student, only the gap or the content
      (`docs/domain-rules/README.md` — "the child is never blamed").
- [ ] Self-report signals (if any) are never treated as ground truth
      (`docs/pedagogy/metacognition.md`).

## Data Integrity (Constitutional)

- [ ] No prompt output is treated as directly setting Confidence or Learning State — any
      confidence-adjacent judgment is a structured signal consumed by deterministic code, per
      `docs/prompt-engineering/learning-state-prompt.md`'s split (ADR-004).
- [ ] No prompt output bypasses Validation before reaching a Student (ADR-005).

## Structure

- [ ] Output shape is fully structured (not free prose) wherever Validation needs to check specific
      fields — a Validation step that has to re-parse prose to extract a verdict is a design smell.
- [ ] The prompt states its `PromptContext` inputs explicitly and takes no implicit inputs
      (`docs/prompt-engineering/generation-engine-prompt.md`).

## Versioning

- [ ] The prompt carries an explicit version identifier (`docs/prompt-engineering/prompt-versioning-strategy.md`)
      and every output records which version produced it.

## Evaluation

- [ ] The prompt has corresponding `testing/golden-dataset.md` / `docs/golden-dataset/structure.md`
      coverage before it's considered ready to ship, per
      `docs/prompt-engineering/prompt-evaluation-metrics.md`.

## Related Documents

Every document in `docs/prompt-engineering/`, `docs/domain-rules/README.md`, ADR-004, ADR-005,
`automation/ai-automation/prompt-evaluation-workflow.md`.
