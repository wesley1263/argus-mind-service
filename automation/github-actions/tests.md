# Workflow: Tests

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches), `push:main`
- **Blocking:** Required check

## Purpose

Runs the full test suite described in `testing/README.md` — unit, integration, contract, and
architecture tests — identically to a local `docker compose run --rm test` invocation.

## Steps

1. Check out the repository.
2. Start the `db` service (and any other real dependency integration tests need — see
   `testing/integration-testing.md`), so integration/contract tests run against real systems, never
   mocks.
3. Run `docker compose run --rm test`.
4. Publish a machine-readable test report (JUnit XML or equivalent) as a CI artifact for
   `coverage.md` and `code-metrics.md` to consume.

## Gate / Threshold

100% of the suite passes. Zero skipped tests without an explicit, reviewed `xfail`/skip reason
attached in code — an unexplained skip is treated as a failure by this workflow's report parser.

## Inputs / Secrets

Test-only database credentials (`skills/github-actions.md` §Secrets) scoped to the ephemeral CI
database instance only.

## Outputs / Artifacts

Pass/fail status check; JUnit-format test report artifact; per-category pass counts (unit /
integration / contract / architecture) surfaced in the job summary.

## Failure Behavior

Blocks merge. A failing architecture test specifically (see
`testing/architecture-testing.md`) is flagged distinctly in the summary, since it indicates a
Constitutional violation (`.ai/constitution.md`), not an ordinary bug.

## Related Docs

`testing/README.md`, `testing/unit-testing.md`, `testing/integration-testing.md`,
`testing/architecture-testing.md`, `skills/docker.md`.
