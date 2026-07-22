# Skill: GitHub Actions (CI)

CI exists to verify, automatically and identically for every change, exactly what
`skills/docker.md` describes doing locally. It must never be a second, divergent definition of
"passing."

## The Core Rule: CI Calls the Same Commands a Developer Would

Every CI job that lints or tests this codebase runs `docker compose run --rm lint` or
`docker compose run --rm test` (or a scoped variant of `test`, e.g. targeting `testing/unit/` — see
`testing/README.md`) — the exact commands from `CLAUDE.md` and `skills/docker.md`. If CI needs a
check that isn't runnable that way locally, the Docker Compose setup is incomplete; fix it there
first, don't bolt a CI-only script on the side. A contributor should always be able to reproduce a
CI failure by running the same one-liner on their own machine.

## Expected Workflow Structure

- **Trigger:** on every pull request, and on push to `main`.
- **Jobs**, each independent so failures are attributable:
  1. `lint` — `docker compose run --rm lint`.
  2. `test` — `docker compose run --rm test`, which per `testing/README.md` includes unit,
     integration, contract, and architecture tests (golden-dataset/prompt-evaluation runs are
     scoped separately — see below).
  3. `golden-dataset-eval` — runs `testing/golden-dataset.md` / `testing/prompt-evaluation.md`
     checks for changes touching Knowledge Engine extraction or Generation Engine content;
     regressions here block merge the same as a failing unit test.
- **Required checks** on `main`: `lint` and `test` (and `golden-dataset-eval` when the change
  touches the paths it covers) must pass before merge — no bypass, matching the Review Checklist's
  stance that boundary/integrity failures are never approved "with a follow-up"
  (`.ai/review-checklist.md`).

## Branch and Merge Conventions

CI enforcement pairs with the branching convention in `CLAUDE.md`: branches are named with a
`feat/`, `test/`, `refact/`, `bugfix/`, `hotfix/`, or `fix/` prefix, and a PR only merges once its
required checks are green and it has passed review against `.ai/review-checklist.md`. CI does not
replace review — it's the fast, automatable subset of the checklist (boundary-violation and
data-integrity architecture tests, contract tests, lint) that shouldn't consume a human or agent
reviewer's attention.

## Caching and Speed

Cache Poetry's dependency install (keyed on `poetry.lock`) and Docker image layers between runs so
CI stays fast enough that "wait for CI" doesn't become a reason to skip it. A slow CI pipeline is a
correctness risk, not just a convenience problem — it's what causes people to start merging without
waiting for it.

## Secrets

CI-provided credentials (database URLs for integration tests, any external API keys Generation
Engine's adapters need) are injected as GitHub Actions secrets into the Compose environment at run
time — never committed, never hardcoded into `docker-compose.yml` or a Dockerfile.
