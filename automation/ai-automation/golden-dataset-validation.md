# Workflow: Golden Dataset Validation

- **Category:** AI Automation
- **Trigger:** `pull_request` (when a file under the golden dataset itself changes, per
  `testing/golden-dataset.md`'s curation process)
- **Blocking:** Required check on dataset-touching PRs

## Purpose

Validates the golden dataset's own integrity before it's trusted as ground truth — a bad dataset
entry silently corrupts every downstream score in `prompt-evaluation-workflow.md` and
`ai-automation/prompt-regression.md`, so the dataset needs its own gate distinct from what it's used
to evaluate.

## Steps

1. Confirm every dataset entry follows `testing/golden-dataset.md`'s required shape: input scenario,
   expected output (or acceptable range), and a stated reason the expected output is correct (that
   document's explicit requirement — "not just the expected value").
2. Confirm no entry was added or changed without the review discipline `golden-dataset.md`
   requires — check that the PR has a subject-matter reviewer's approval, not just an engineering
   approval, for entries in Knowledge Engine's extraction dataset specifically (where correctness is
   a domain-expertise judgment, not a code-correctness one).
3. Run a consistency check: do any two entries contradict each other (same input scenario, different
   expected output) — this catches copy-paste drift as the dataset grows.
4. Confirm dataset entries are versioned against the model/prompt/extraction-technique version they
   evaluate (`testing/golden-dataset.md`'s versioning requirement).

## Gate / Threshold

Zero malformed entries, zero missing subject-matter review on Knowledge-Engine-extraction entries,
zero direct contradictions.

## Inputs / Secrets

None beyond standard repo read access.

## Outputs / Artifacts

Dataset integrity report; flags on any entry needing subject-matter re-review.

## Failure Behavior

Blocks merge of the dataset change. This is deliberately stricter than
`prompt-evaluation-workflow.md`'s own gate, because a corrupted dataset would silently weaken every
future prompt evaluation, not just the current PR.

## Related Docs

`testing/golden-dataset.md`, `automation/ai-automation/prompt-evaluation-workflow.md`.
