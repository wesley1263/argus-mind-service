# Workflow: Changelog

- **Category:** Automation
- **Trigger:** `push:main` (append-only accumulation of unreleased entries), invoked by
  `github-actions/release.md` at release time to finalize a version section.
- **Blocking:** Advisory (a missing entry is caught by `github-actions/documentation.md`'s PR-level
  check, not by this workflow)

## Purpose

Keeps `CHANGELOG.md` (Keep a Changelog format, `templates/release.md`) accurate and current with
minimal manual upkeep, sourcing entries from PR metadata rather than trusting every contributor to
hand-edit the file correctly.

## Steps

1. On merge to `main`, read the merged PR's description for its Changelog section (the PR template
   requires one — `.github/PULL_REQUEST_TEMPLATE.md`).
2. Classify the entry into Added / Changed / Deprecated / Removed / Fixed / Security based on the
   PR's branch prefix (`feat/`, `fix/`, `hotfix/`, etc. — `CLAUDE.md`) and labels, falling back to
   asking for manual classification if ambiguous.
3. Append the entry under an `[Unreleased]` heading in `CHANGELOG.md`, committed directly to `main`
   by the automation (not requiring a separate human-authored commit for changelog upkeep).
4. At release time (`github-actions/release.md`), rename `[Unreleased]` to the concrete version
   number and date, and open a fresh empty `[Unreleased]` section for what comes next.

## Gate / Threshold

N/A directly — but a release (`github-actions/release.md`) refuses to proceed if `[Unreleased]` is
empty, since that means nothing has actually changed since the last release.

## Inputs / Secrets

Write access to commit directly to `main` (scoped narrowly to this automation, not a general-purpose
credential).

## Outputs / Artifacts

Updated `CHANGELOG.md`, committed automatically.

## Failure Behavior

If classification is ambiguous, the entry is added under `[Unreleased]` with a
`<!-- needs classification -->` marker rather than silently guessing wrong — a human resolves it
before the next release finalizes that section.

## Related Docs

`templates/release.md`, `.github/PULL_REQUEST_TEMPLATE.md`, `automation/github-actions/release.md`,
`automation/workflows/release-notes.md`.
