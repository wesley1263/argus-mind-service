# Golden Dataset

The domain-level specification of Smart App's golden dataset — what it contains and how it's
evaluated. `testing/golden-dataset.md` (Phase 2) covers the CI mechanics (curation governance,
versioning, how it gates merges); this folder covers the pedagogical content and rubric.

| Document | Covers |
|---|---|
| [`structure.md`](structure.md) | The schema and a worked example for each of the ten item types — Example Chapters, Expected Learning Nodes, Session Plans, Mind Maps, Summaries, Feedback, Confidence, Regeneration, Games, and Quizzes. |
| [`evaluation-methodology.md`](evaluation-methodology.md) | How each item type is actually scored, and who reviews new entries. |

## Why This Dataset Exists

Every prompt in `docs/prompt-engineering/` and every Engine's extraction/computation logic needs a
concrete, human-reviewed standard to be checked against — without one, "is this good?" has no answer
beyond an engineer's own judgment in the moment. This dataset is that standard, made concrete enough
to run in CI (`automation/ai-automation/`) and specific enough to reflect this platform's actual
pedagogy, not generic content-quality heuristics.

## Related Documents

`testing/golden-dataset.md`, `docs/prompt-engineering/README.md`,
`automation/ai-automation/golden-dataset-validation.md`.
