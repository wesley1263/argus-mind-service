# AI Automation

Automation specific to the platform's generative-AI-adjacent surfaces: Generation Engine's prompts
and model calls, and Knowledge Engine's extracted content. These build directly on Phase 2's
`testing/prompt-evaluation.md` and `testing/golden-dataset.md` — this folder is *where those
methodologies actually run*, on a schedule and in CI, rather than being invoked manually.

## Index

| Workflow | Trigger | Purpose |
|---|---|---|
| [`prompt-evaluation-workflow.md`](prompt-evaluation-workflow.md) | Prompt/model config PR | Gates a prompt change against the golden dataset rubric. |
| [`golden-dataset-validation.md`](golden-dataset-validation.md) | Dataset PR | Validates the golden dataset's own integrity before it's trusted. |
| [`prompt-regression.md`](prompt-regression.md) | Nightly | Catches silent drift in the live model provider, not just our own prompt changes. |
| [`llm-cost-monitoring.md`](llm-cost-monitoring.md) | Daily + real-time | Tracks generative-model spend against budget, with a hard circuit breaker. |
| [`knowledge-base-validation.md`](knowledge-base-validation.md) | Nightly | Validates the accumulated Knowledge Graph, not just each new Document's extraction. |

## Why This Is Separate From `github-actions/`

Every workflow here evaluates something *probabilistic* — model output quality, extraction quality,
cost that varies with usage — rather than something deterministically pass/fail like a lint rule.
They still gate merges where appropriate (`prompt-evaluation-workflow.md`,
`golden-dataset-validation.md`), but their thresholds are calibrated quality scores and budgets, not
binary correctness checks, and several of them run on a schedule independent of any PR because the
thing they're watching (a vendor's model behavior, accumulated Knowledge Graph state) can change
without any commit to this repository at all.

## Relationship to Article I of the Constitution

`.ai/constitution.md` Article I insists the AI technique stay swappable. This folder is what makes
that a checkable claim rather than an aspiration: cost, quality, and drift are all measured, so a
decision to swap providers or techniques is made from data, not from bias toward whatever is
currently wired up.
