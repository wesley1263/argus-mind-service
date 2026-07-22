# Workflow: Repository Health

- **Category:** Automation
- **Trigger:** `schedule` (weekly)
- **Blocking:** N/A

## Purpose

Rolls up every other automation's output into one weekly signal of the repository's overall state —
the thing a lead engineer (human or AI) reads first to know whether anything needs attention, rather
than checking a dozen workflows individually.

## Steps

1. Aggregate the latest results from: `github-actions/coverage.md` (trend), `workflows/code-metrics.md`
   (complexity/size trend), `github-actions/dead-code-detection.md` and
   `github-actions/unused-dependency-detection.md` (accumulated advisory findings),
   `github-actions/spec-drift-check.md` (open drift findings), `ai-automation/prompt-regression.md`
   and `ai-automation/golden-dataset-validation.md` (quality-score trend), `ai-automation/llm-cost-monitoring.md`
   (spend trend), open Dependabot-style PRs from `workflows/dependency-updates.md`.
2. Compute a small set of headline indicators (not a single opaque "health score"): days since last
   release, count of stale spec-drift findings, coverage trend direction, open Critical/High
   dependency findings, golden-dataset score trend direction.
3. Post the summary to wherever the team/agent roster actually looks (a pinned issue, a repository
   discussion, or a dashboard update — see `documentation-automation/repository-dashboard.md` for
   the always-current version of this same data).

## Gate / Threshold

N/A — this is a digest, not a gate. If a threshold matters, it's already gating in the workflow that
owns it (`quality-gates.md`, `github-actions/dependency-scan.md`, etc.); this workflow just makes
the aggregate visible.

## Inputs / Secrets

Read access to every workflow's stored output/artifacts.

## Outputs / Artifacts

Weekly health summary (issue update or discussion post) with headline indicators, each linked to
its source workflow for drill-down.

## Failure Behavior

If a source workflow's data is unavailable, that indicator is reported as "no data this week"
rather than omitted silently or defaulted to a misleadingly clean value.

## Related Docs

Every workflow in `automation/github-actions/`, `automation/ai-automation/`, and
`automation/workflows/`; `automation/documentation-automation/repository-dashboard.md`.
