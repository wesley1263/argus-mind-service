# Golden Dataset: Evaluation Methodology

How each `structure.md` item type is actually scored — the pedagogical rubric behind
`testing/golden-dataset.md`'s CI mechanics and `docs/prompt-engineering/prompt-evaluation-metrics.md`'s
dimensions, applied per item type.

## General Method

Every item type is scored by comparing a system-produced output against the item's human-reviewed
expected structure — never by exact string match (which would penalize valid rephrasing) and never
by pure semantic similarity alone (which would miss structural errors like a fabricated Knowledge
Edge). The comparison method is specific to what each item type actually needs to get right:

| Item Type | Comparison Method |
|---|---|
| Expected Learning Nodes | Structural: does node count, granularity, and prerequisite direction match the reviewed expectation, per `docs/pedagogy/chunking.md`'s granularity standard? |
| Expected Session Plans | Structural: does pacing/format-variety match `docs/domain/study-session.md`'s shape? |
| Expected Mind Maps | Structural: do connections correspond to real Knowledge Edges (`docs/prompt-engineering/mindmap-validation-prompt.md`)? |
| Expected Summaries | Rubric-scored on completeness/correctness/jargon-detection (`docs/prompt-engineering/summary-evaluation-prompt.md`) against multi-tier reviewed examples. |
| Expected Feedback | Rubric-scored on localization and tone (`docs/domain-rules/README.md`). |
| Expected Confidence | Numeric: does the computed trajectory fall within a reviewed acceptable range, including the decay and false-negative-risk scenarios? |
| Expected Regeneration | Structural: is regeneration scope correctly limited to the problematic Learning Node? |
| Expected Games | Structural: element grounding and mechanic well-formedness. |
| Expected Quizzes | Structural: Bloom's-level match (`docs/pedagogy/bloom-taxonomy.md`). |

## Who Reviews New Entries

Knowledge Engine extraction entries (Learning Nodes, Knowledge Edges) require a subject-matter
reviewer's approval, not just an engineering approval — per
`automation/ai-automation/golden-dataset-validation.md`'s review-discipline check. Generation Engine
content entries (Summaries, Feedback, Quizzes) benefit from a reviewer with pedagogical background,
though this is a strong recommendation rather than a hard CI-enforced gate.

## Handling Disagreement Among Reviewers

Where two subject-matter reviewers reasonably disagree on an expected value (e.g., whether concept A
is truly a prerequisite of concept B), the dataset entry should record the *range* of acceptable
answers explicitly, rather than forcing a single reviewer's judgment to become unquestioned ground
truth — this keeps the dataset honest about genuine pedagogical judgment calls versus objective
correctness.

## Related Documents

`docs/golden-dataset/structure.md`, `testing/golden-dataset.md`,
`docs/prompt-engineering/prompt-evaluation-metrics.md`,
`automation/ai-automation/golden-dataset-validation.md`.
