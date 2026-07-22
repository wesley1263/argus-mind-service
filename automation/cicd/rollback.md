# CI/CD: Rollback

## Purpose

Defines how the platform recovers from a bad Production deploy — fast, and without violating any of
the data-integrity guarantees (`.ai/constitution.md` Article IV) a naive rollback could threaten.

## Two Kinds of Rollback

### 1. Traffic Rollback (fast path)

During or shortly after a Blue-Green cutover (`blue-green.md`), while Blue is still running idle:
shift traffic back to Blue immediately. This is the default, preferred rollback — it's a routing
change, not a redeploy, and completes in the time it takes traffic to shift, the same mechanism as
the forward cutover.

- **Automatic trigger**: `github-actions/deploy.md`'s post-deploy smoke test fails, or an elevated
  error-rate/latency alert fires within the bake period.
- **Manual trigger**: `workflow_dispatch`, for a problem the automated smoke test didn't catch.

### 2. Version Rollback (after Blue has been retired)

If the problem is discovered after Blue has already been torn down, rollback means redeploying the
previous known-good tagged release through the normal `deploy.md` path (treating the old version as
the new Green) — not a special-cased "undo," so it goes through the same smoke-tested, Blue-Green
promotion path as any other deploy.

## What Rollback Does *Not* Undo

Per ADR-003/ADR-004, Evidence and Learning State are never deleted or rewritten to "undo" what
happened while the bad version was live — rolling back the application version does not roll back
recorded Evidence. If the bad version itself recorded incorrect Evidence (e.g., a bug that mislabeled
which Learning Node an answer applied to), that's a data-quality incident handled by recording
**corrective Evidence** (per `specs/evidence-engine.md` R4), never by mutating or deleting the
original records — a code rollback and a data correction are two different operations, and this
spec deliberately keeps them separate so "roll back the app" is never mistaken for "the data is now
clean."

## Database Migrations and Rollback

Because Blue-Green requires migrations to be backward-compatible during the overlap window
(`blue-green.md`), a traffic rollback to Blue never requires a database rollback — Blue's queries
already work against the current schema. A version rollback to a much older release, spanning
migrations that were *not* designed with this overlap discipline (pre-dating this CI/CD setup), is
explicitly out of scope for automatic handling and requires manual DBA judgment.

## Gate / Threshold

Traffic rollback completes within the same latency as a normal cutover. Any Production incident
triggering rollback is logged with enough detail to become a `templates/bug.md` entry, checked for
the Constitutional-integrity boxes in that template.

## Related Docs

`automation/cicd/blue-green.md`, `automation/cicd/environments.md`, ADR-003, ADR-004,
`specs/evidence-engine.md`, `templates/bug.md`.
