# Workflow: Deploy

- **Category:** GitHub Actions
- **Trigger:** `push:main` (auto-deploy to Development), GitHub Release published (Staging), manual
  `workflow_dispatch` with approval gate (Production) — see `cicd/environments.md` for why each
  environment has a different trigger.
- **Blocking:** N/A for Development/Staging (they gate nothing downstream automatically); Production
  requires manual approval as its own gate.

## Purpose

Ships a built, validated image (from `docker-build.md`) to a specific environment, using the
environment topology and promotion rules in `cicd/environments.md`.

## Steps

1. Confirm the target image passed `docker-build.md` and, for Staging/Production, that it
   corresponds to a tagged Release from `release.md` — Development may deploy directly from `main`
   without a formal release.
2. Apply environment-specific configuration (secrets, connection strings) injected at deploy time,
   never baked into the image (`skills/docker.md`, `skills/github-actions.md` §Secrets).
3. Run database migrations for that environment (`skills/postgres.md`) before traffic is shifted to
   the new version — a migration failure aborts the deploy before any traffic moves.
4. For Production specifically: perform the deploy via the Blue-Green strategy in
   `cicd/blue-green.md`, not a direct in-place replacement.
5. Run a post-deploy smoke test (the same health check as `docker-build.md`'s, plus one real
   round-trip through `specs/study-session.md`'s API against the newly deployed environment).
6. On smoke test failure in Production, automatically trigger `cicd/rollback.md`.

## Gate / Threshold

Migrations must apply cleanly and the post-deploy smoke test must pass before traffic fully shifts
(Production); Development/Staging fail loudly but do not require manual sign-off to retry.

## Inputs / Secrets

Per-environment credentials and connection strings (`cicd/environments.md`), deploy target
credentials, scoped separately per environment so a Development credential can never deploy to
Production.

## Outputs / Artifacts

Deployed environment; deployment record (what version, when, by what triggered it) feeding
`workflows/repository-health.md` and `documentation-automation/repository-dashboard.md`.

## Failure Behavior

Development/Staging: fails the workflow, alerts, environment stays on its last good version.
Production: automatic rollback (`cicd/rollback.md`) is triggered without waiting for a human,
because Production downtime is the more expensive failure mode.

## Related Docs

`automation/cicd/environments.md`, `automation/cicd/blue-green.md`, `automation/cicd/rollback.md`,
`specs/study-session.md`.
