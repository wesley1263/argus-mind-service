# Workflow: Code Metrics

- **Category:** Automation
- **Trigger:** `push:main` (trend tracking), `pull_request` (delta reporting)
- **Blocking:** N/A (feeds `quality-gates.md`'s Complexity/Cyclomatic Complexity gates, which are
  what actually block)

## Purpose

Tracks the codebase's structural health over time — complexity, size, coverage, dead-code and
unused-dependency trends — so degradation is visible as a trend long before any single PR is bad
enough to fail a gate on its own.

## Steps

1. Compute per-Engine metrics on every `main` push: cyclomatic complexity distribution (see
   `quality-gates.md`), lines of code per layer (`domain`/`application`/`ports`/`adapters` —
   a growing `adapters/` relative to `domain/` can indicate business logic leaking into the edge,
   per `skills/clean-architecture.md`), test count and coverage (`github-actions/coverage.md`),
   accumulated dead-code/unused-dependency findings (`github-actions/dead-code-detection.md`,
   `github-actions/unused-dependency-detection.md`).
2. On PR, compute the same metrics' delta against the base branch and post it as a comment —
   informational, not gating (the gating versions of complexity live in `quality-gates.md`).
3. Persist the `main`-push metrics as a time series for `workflows/repository-health.md` and
   `documentation-automation/repository-dashboard.md` to render as trend charts.

## Gate / Threshold

N/A directly. This workflow's job is measurement; `quality-gates.md` defines what's blocking.

## Inputs / Secrets

Write access to persist the metrics time series (a lightweight data store or the repository's own
metrics-tracking mechanism).

## Outputs / Artifacts

Time-series metrics data; PR delta comments; dashboard feed.

## Failure Behavior

A metrics-collection failure is logged and skipped for that run (the time series has a gap) rather
than blocking anything — this is an observability workflow, not a gate.

## Related Docs

`automation/quality-gates.md`, `automation/workflows/repository-health.md`,
`automation/documentation-automation/repository-dashboard.md`, `skills/clean-architecture.md`.
