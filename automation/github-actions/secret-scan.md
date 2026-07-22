# Workflow: Secret Scan

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches, full diff), `push` (all branches)
- **Blocking:** Required check

## Purpose

Prevents a credential, API key, or token from ever landing in the repository's history, per
`skills/prompt-engineering.md` §Secrets, `skills/github-actions.md` §Secrets, and
`.ai/review-checklist.md`'s implicit trust that nothing sensitive is committed.

## Steps

1. Scan the full diff (not just changed-file contents, but the actual added lines, to catch a
   secret added and then partially edited) using a pattern- and entropy-based secret scanner (e.g.
   `gitleaks` or `trufflehog`) with a maintained ruleset covering common credential formats and this
   project's own known secret shapes (DB connection strings, model-vendor API keys).
2. On `push` to any branch, additionally scan the incoming commit range so a secret can't be
   introduced by a direct push that bypasses PR review.
3. If a finding is confirmed real (not a false-positive pattern like an example key in
   documentation), immediately flag it as a security incident, not just a failed check — see
   Failure Behavior.

## Gate / Threshold

Zero findings. There is no severity tiering here — any matched secret pattern blocks, because the
cost of a false positive (an author double-checks and adds an allowlist entry for a genuinely safe
match, e.g. a documented example key in `skills/prompt-engineering.md` explicitly marked as a
placeholder) is far lower than the cost of one real leaked credential.

## Inputs / Secrets

None — ironically, this workflow needs no secrets of its own.

## Outputs / Artifacts

Pass/fail status check; on a real finding, an alert to whoever owns credential rotation (not just a
failed CI check that a contributor might route around).

## Failure Behavior

Blocks merge/push immediately. If a secret is found already merged to `main` (caught by a
retroactive scan), treat it as **compromised** — rotate the credential, don't just remove it from
the file, since git history retains it regardless.

## Related Docs

`skills/docker.md`, `skills/github-actions.md` §Secrets, `skills/prompt-engineering.md` §Secrets.
