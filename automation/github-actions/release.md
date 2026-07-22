# Workflow: Release

- **Category:** GitHub Actions
- **Trigger:** `push:main` (evaluates whether a release is warranted), or `workflow_dispatch`
  (manual trigger for a deliberate release)
- **Blocking:** N/A (this workflow *produces* a release; it doesn't gate a PR)

## Purpose

Turns a green `main` into a versioned, tagged release with generated release notes, following
`templates/release.md`'s Keep a Changelog shape and `cicd/versioning-and-tagging.md`'s scheme —
without a human manually computing a version number or writing notes from memory.

## Steps

1. Confirm `main`'s latest commit has passed `lint.md`, `tests.md`, `coverage.md`, and
   `docker-build.md` — a release never runs against a commit that hasn't already passed every
   required check.
2. Determine the next version number per `cicd/versioning-and-tagging.md` (semantic versioning;
   major bump for any breaking Engine contract change per `.ai/development-principles.md` §4).
3. Compile the release's `CHANGELOG.md` entry from merged PRs since the last release
   (`workflows/changelog.md` and `workflows/release-notes.md` are the two automations that feed
   this step — see those specs for how entries are sourced).
4. Create a git tag and a GitHub Release using that changelog entry as the release body.
5. Trigger `docker-build.md`'s release-tagged build and `deploy.md` for the appropriate environment
   (`cicd/environments.md`).

## Gate / Threshold

Only runs against a commit where every required check is green. Refuses to release if
`workflows/changelog.md` produces no entries since the last release (nothing to release) rather
than publishing an empty release.

## Inputs / Secrets

Write access to create tags/releases; registry credentials passed through to the triggered
`docker-build.md`/`deploy.md` runs.

## Outputs / Artifacts

Git tag, GitHub Release with generated notes, updated `CHANGELOG.md` committed back to `main`.

## Failure Behavior

Aborts without creating a partial/broken release. A failure here pages whoever owns releases rather
than silently retrying — a half-completed release (tagged but not deployed, or deployed but not
tagged) is worse than a delayed one.

## Related Docs

`templates/release.md`, `automation/workflows/changelog.md`,
`automation/workflows/release-notes.md`, `automation/cicd/versioning-and-tagging.md`.
