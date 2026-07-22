# Workflow: Versioning

- **Category:** GitHub Actions
- **Trigger:** `push:main` (computes and validates the next version on every merge, without
  necessarily releasing it — see `release.md` for the release trigger itself)
- **Blocking:** Advisory on regular merges; Required check specifically on any PR that touches a
  published Engine contract (`.ai/architecture.md` §4)

## Purpose

Computes what the *next* semantic version would be, and — critically — validates that a PR's
declared change type (additive vs. breaking, per `.ai/development-principles.md` §4) matches what
it actually does, before that mismatch becomes a released version number nobody can trust.

## Steps

1. Inspect merged commit messages/PR labels since the last release for conventional change markers
   (feature, fix, breaking-change) — see `cicd/versioning-and-tagging.md` for the exact scheme.
2. Cross-check any PR labeled "breaking" against `automation/github-actions/specification-validation.md`'s
   contract-diff output: does the diff actually remove a field, narrow a type, or otherwise break an
   existing consumer per `.ai/development-principles.md` §4? If a PR is labeled non-breaking but the
   contract diff says otherwise, fail this check — the label, not just the code, must be honest.
3. Compute the candidate next version (major/minor/patch) and publish it as a check output other
   workflows (`release.md`) can consume.

## Gate / Threshold

A PR that changes a published contract must carry a label matching what the automated contract-diff
detected (breaking vs. additive). A mismatch fails the check on PRs touching a contract; it does
not fail unrelated PRs.

## Inputs / Secrets

None beyond read access to commit history and PR metadata.

## Outputs / Artifacts

Computed candidate version number (consumed by `release.md`); contract-change classification
report.

## Failure Behavior

Blocks merge only for contract-touching PRs with a label/reality mismatch — the fix is to correct
the label (or, if the change is genuinely breaking, to follow
`.ai/development-principles.md` §4's consumer-migration path, possibly requiring an ADR).

## Related Docs

`automation/cicd/versioning-and-tagging.md`, `automation/github-actions/release.md`,
`automation/github-actions/specification-validation.md`, `.ai/development-principles.md` §4.
