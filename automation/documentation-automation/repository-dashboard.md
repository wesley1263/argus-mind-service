# Workflow: Repository Dashboard

- **Category:** Documentation Automation
- **Trigger:** `schedule` (aligned with `workflows/repository-health.md`'s weekly cadence, plus
  incremental updates on every `push:main` for the fast-moving indicators)
- **Blocking:** N/A

## Purpose

`workflows/repository-health.md` produces a periodic digest (posted to an issue/discussion); this
workflow maintains the **always-current, browsable version** of the same underlying data — a single
page anyone (or any agent starting a new session with no memory of prior conversations) can open to
understand the repository's current state without re-deriving it from a dozen individual workflow
runs.

## Steps

1. Pull the latest data from every source `repository-health.md` aggregates: coverage trend
   (`github-actions/coverage.md`), complexity/size trend (`workflows/code-metrics.md`), open
   spec-drift findings (`github-actions/spec-drift-check.md`), golden-dataset/prompt-evaluation
   score trend (`ai-automation/`), LLM cost trend (`ai-automation/llm-cost-monitoring.md`), spec
   status counts (`documentation-automation/specification-index.md`), ADR status counts
   (`documentation-automation/adr-index.md`), open dependency-update PRs
   (`workflows/dependency-updates.md`).
2. Render as a single markdown page (or a small set of linked pages) with trend charts where the
   hosting mechanism supports rendering them, and plain tables otherwise — markdown-renderable
   either way, per Phase 3's constraint.
3. Regenerate on every `push:main` for indicators cheap to compute (spec/ADR counts) and on the
   weekly schedule for the more expensive aggregations (full metrics/cost trends), so the dashboard
   is never more than a few minutes stale for the things that change fastest.

## Gate / Threshold

N/A — purely generative/observability.

## Inputs / Secrets

Read access to every source workflow's stored output; write access to publish the dashboard page.

## Outputs / Artifacts

A single always-current dashboard page, linked from the root `README.md`.

## Failure Behavior

If a source is temporarily unavailable, its section renders "no data" rather than stale data
silently presented as current — the same honesty principle `workflows/repository-health.md` applies
to its own digest.

## Related Docs

`automation/workflows/repository-health.md`, every workflow it aggregates,
`automation/documentation-automation/architecture-index.md`.
