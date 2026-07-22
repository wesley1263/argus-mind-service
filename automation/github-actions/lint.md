# Workflow: Lint

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches), `push:main`
- **Blocking:** Required check

## Purpose

Enforces `skills/python.md`'s toolchain rules (Ruff + mypy strict) mechanically, on every change,
identically to what a contributor runs locally.

## Steps

1. Check out the repository.
2. Run `docker compose run --rm lint`, exactly as specified in `CLAUDE.md` and
   `skills/docker.md` — no separate CI-only lint invocation.
3. That service internally runs Ruff (lint + format check, `--check` mode — never auto-fixing in
   CI) and mypy in strict mode across the whole codebase.
4. Surface both tools' output as CI annotations inline on the changed lines where the CI provider
   supports it.

## Gate / Threshold

Zero Ruff violations, zero mypy errors. No warning-only mode — a lint or type error fails the
check (`.ai/coding-philosophy.md`: typing is not optional).

## Inputs / Secrets

None — lint runs fully offline against the checked-out code.

## Outputs / Artifacts

Pass/fail status check; annotated diagnostics on the PR diff.

## Failure Behavior

Blocks merge. No override path — per `.ai/review-checklist.md`, code-quality and typing issues are
fixed, not waived.

## Related Docs

`skills/python.md`, `skills/docker.md`, `CLAUDE.md`.
