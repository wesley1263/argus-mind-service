# Workflow: Prompt Evaluation

- **Category:** AI Automation
- **Trigger:** `pull_request` (when a Generation Engine prompt template or model configuration
  changes, per `skills/prompt-engineering.md`)
- **Blocking:** Required check on prompt-touching PRs

## Purpose

Runs `testing/prompt-evaluation.md`'s rubric-scored methodology in CI, automatically, on every
prompt change — the automated gate that makes "a prompt change is evaluated before it ships" (that
document's stated rule) actually enforced rather than aspirational.

## Steps

1. Detect the changed prompt template/version (`skills/prompt-engineering.md`'s versioning
   requirement makes this detectable — a new version identifier appears).
2. Run the new prompt against every scenario in the relevant `golden-dataset.md` dataset.
3. Score each output along `testing/prompt-evaluation.md`'s five dimensions (Correctness,
   Difficulty match, Pedagogical soundness, Safety, Language/tone consistency).
4. Compare dimension-by-dimension against the currently-active prompt's last recorded scores
   (`ai-automation/prompt-regression.md` performs the deeper regression-specific comparison; this
   workflow performs the initial scoring run that feeds it).
5. Post a per-dimension, per-scenario report as a PR comment — never a single composite score
   (`testing/prompt-evaluation.md`'s explicit stance against a meaningless composite number).

## Gate / Threshold

No dimension may regress by more than the threshold configured in `ai-automation/prompt-regression.md`.
A first-time prompt (no prior baseline) requires an explicit human-reviewed minimum score per
dimension rather than skipping the gate for lack of a comparison point.

## Inputs / Secrets

Credentials for the generative model provider used to produce evaluation outputs
(`skills/prompt-engineering.md` §Secrets); credentials for the scoring judge if a model-based judge
is used (`testing/prompt-evaluation.md`'s note on periodically spot-checking a model judge against
human review).

## Outputs / Artifacts

Per-dimension score report; scored outputs archived for audit alongside the prompt version that
produced them.

## Failure Behavior

Blocks merge on a regression past threshold. A flat "it's worse" report is not delivered without the
specific regressed scenarios attached — a reviewer must be able to see *what* got worse, not just
that something did.

## Related Docs

`testing/prompt-evaluation.md`, `testing/golden-dataset.md`, `skills/prompt-engineering.md`,
`automation/ai-automation/prompt-regression.md`.
