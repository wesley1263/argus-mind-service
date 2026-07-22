# Workflow: Spec Drift Check

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (when implementation code changes without a corresponding `specs/`
  change), `schedule` (weekly, full-repo drift audit)
- **Blocking:** Required check on PR (for the specific drift types below); advisory on the scheduled
  full audit

## Purpose

`specification-validation.md` checks that a spec is well-formed on its own; this workflow checks
that a spec and the *implementation* built from it haven't quietly diverged over time — the
automation behind `.ai/development-principles.md` §9's "documentation drift is a bug" rule, applied
specifically to specs.

## Steps

1. For each spec with Status `Implemented`, extract its API section's declared endpoints and compare
   them against the actual routes registered in the corresponding Engine's FastAPI router
   (`skills/fastapi.md`) — flag any route that exists in code but not in the spec, or vice versa.
2. Compare each spec's Database section against the actual current schema (introspected from
   applied migrations, `skills/postgres.md`) for the tables it declares owning.
3. Compare each spec's Events section against the actual event names/payloads produced and consumed
   in code.
4. On the weekly scheduled run, perform all three checks across every `Implemented` spec, not just
   ones touched by a recent PR, and file/update a tracking issue (via `templates/bug.md`'s shape,
   tagged `spec-drift`) for anything found — implementation can drift without any single PR being
   "at fault," e.g. through a series of small, individually-reasonable changes.

## Gate / Threshold

PR-time: zero drift introduced by the PR itself between its own code changes and the spec it claims
to implement. Scheduled: zero *open* drift findings older than 30 days — an open finding past that
age escalates from advisory to blocking the next unrelated PR touching that Engine, so drift can't
be indefinitely ignored.

## Inputs / Secrets

Read access to the deployed/CI database schema for introspection (`skills/postgres.md`).

## Outputs / Artifacts

Drift report (API/Database/Events, per spec); scheduled-run tracking issues.

## Failure Behavior

PR-time drift blocks that PR. Escalated stale drift (30+ days) blocks the next PR touching the
affected Engine until either the spec or the code is corrected to match — deliberately forcing
resolution rather than letting drift accumulate indefinitely.

## Related Docs

`specs/README.md`, `.ai/development-principles.md` §9, `templates/bug.md`,
`automation/github-actions/specification-validation.md`.
