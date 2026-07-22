# Workflow: Coverage

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches)
- **Blocking:** Required check

## Purpose

Measures test coverage produced by `tests.md`'s run and blocks a change that reduces it below the
project floor — a mechanical backstop for `.ai/definition-of-done.md`'s "tests exist for the
new/changed behavior" requirement, not a replacement for judging whether the tests are meaningful
(`skills/testing.md`).

## Steps

1. Reuse the test run from `tests.md` (do not re-run the suite a second time in a separate job —
   collect coverage in the same invocation via the test runner's coverage plugin).
2. Compute overall coverage percentage and per-file/per-Engine coverage.
3. Compute the coverage delta against the target branch.
4. Post a summary comment on the PR: overall %, delta, and any file that dropped below the floor.

## Gate / Threshold

- Overall coverage must not drop below **85%** (an initial floor — track via
  `templates/decision.md` if this needs revisiting once real usage data exists).
- No individual changed file may be **below 80%** coverage on its own changed lines ("patch
  coverage"), even if overall coverage is fine — this catches a well-covered codebase hiding one
  under-tested new file.
- `domain/` and `application/` code (per `skills/clean-architecture.md`) is held to a stricter
  **95%** floor, since it's pure and cheap to test fully (`testing/unit-testing.md`); `adapters/`
  code is held to the 80% patch floor.

## Inputs / Secrets

None beyond the test run's own (`tests.md`).

## Outputs / Artifacts

Coverage report artifact (HTML + machine-readable); PR comment summary; badge data for
`documentation-automation/repository-dashboard.md`.

## Failure Behavior

Blocks merge. Coverage regressions are reported with the specific uncovered lines, not just a
percentage, so a reviewer can judge in seconds whether the gap is meaningful.

## Related Docs

`automation/github-actions/tests.md`, `testing/unit-testing.md`, `.ai/definition-of-done.md`.
