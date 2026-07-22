# Workflow: Dependency Updates

- **Category:** Automation
- **Trigger:** `schedule` (weekly)
- **Blocking:** N/A (opens PRs that go through the normal Required-check gates)

## Purpose

Keeps dependencies current proactively (e.g. via Dependabot/Renovate-style automation), so version
upgrades happen in small, reviewable increments instead of large, risky batch upgrades once a
dependency scan (`github-actions/dependency-scan.md`) finally forces the issue.

## Steps

1. Check `poetry.lock` against upstream for available minor/patch updates (major version bumps are
   opened separately and labeled distinctly — they're more likely to be breaking and deserve
   deliberate review, not routine auto-merge consideration).
2. Group related updates into one PR where they're tightly coupled (e.g. a framework and its
   official plugin), and keep unrelated updates in separate PRs so a single failing check doesn't
   block unrelated upgrades.
3. Open each PR following the standard branch-prefix convention (`CLAUDE.md`) — `fix/` for
   patch-level, `feat/` is not appropriate here since no feature is added; use a dedicated
   `deps/` prefix instead, documented as an accepted exception to `CLAUDE.md`'s listed prefixes.
4. Run the full standard Required-check suite (`github-actions/`) on every dependency-update PR —
   no shortcut path.

## Gate / Threshold

Minor/patch updates with all Required checks green may be configured for auto-merge; major version
updates always require human review regardless of check status.

## Inputs / Secrets

Write access to open PRs and branches.

## Outputs / Artifacts

Opened PRs, one (or a small coupled group) per dependency update.

## Failure Behavior

A failing Required check on an update PR blocks its merge like any other PR — it does not retry
silently or downgrade the check to advisory just because the PR was bot-generated.

## Related Docs

`automation/github-actions/dependency-scan.md`,
`automation/github-actions/unused-dependency-detection.md`, `CLAUDE.md`.
