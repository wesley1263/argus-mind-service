# Workflow: Release Notes

- **Category:** Automation
- **Trigger:** Invoked by `github-actions/release.md` as a sub-step; also runnable via
  `workflow_dispatch` to preview notes before cutting a release.
- **Blocking:** N/A

## Purpose

Generates the human-facing prose that accompanies a GitHub Release — distinct from
`changelog.md`'s job of maintaining the structured `CHANGELOG.md` file itself. Release notes are
narrative and may group/highlight differently than the changelog's strict Keep a Changelog
categories (e.g., leading with the single most significant change).

## Steps

1. Take `changelog.md`'s generated entries for the version being released as the factual base — this
   workflow never invents content the changelog doesn't already substantiate.
2. Group entries by user-facing theme rather than strictly by Added/Changed/Fixed/Removed/Security
   (e.g., "Adaptive loop improvements," "Ingestion reliability") when multiple entries share a
   theme, to make the notes readable rather than a flat list.
3. Surface any Security entries first, unconditionally, regardless of thematic grouping.
4. Link each highlighted item back to its spec (`specs/*.md`) or ADR where applicable, so notes are
   traceable to the source of truth, not just prose.

## Gate / Threshold

N/A — this produces content for human review before `release.md` publishes it, not a pass/fail
check.

## Inputs / Secrets

None beyond what `changelog.md` already gathered.

## Outputs / Artifacts

Draft release-notes markdown, attached to the GitHub Release created by `github-actions/release.md`.

## Failure Behavior

If generation fails, `release.md` falls back to publishing the raw `CHANGELOG.md` entries verbatim
rather than blocking the release on prose quality.

## Related Docs

`automation/workflows/changelog.md`, `automation/github-actions/release.md`, `templates/release.md`.
