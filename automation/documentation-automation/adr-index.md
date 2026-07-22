# Workflow: ADR Index

- **Category:** Documentation Automation
- **Trigger:** `pull_request` (when any file under `adr/` changes)
- **Blocking:** Required check (index consistency only — not a judgment on the ADR's content)

## Purpose

Keeps `adr/README.md`'s index table in sync with the actual ADR files on disk, so the index is never
missing an ADR or listing a stale status — the same trust problem `github-actions/spec-drift-check.md`
solves for specs, applied to ADRs.

## Steps

1. Scan `adr/` for every `ADR-*.md` file (excluding `template.md` and `README.md`).
2. Parse each file's title and `Status:` field.
3. Regenerate `adr/README.md`'s index table from that scan and diff it against the committed
   version.
4. If the PR changed an ADR's status or added a new ADR without updating the index in the same PR,
   fail the check with the exact rows that are out of sync — or, configured as an alternative mode,
   auto-commit the regenerated table back onto the PR branch so the author never has to hand-edit it.

## Gate / Threshold

Zero drift between `adr/README.md`'s table and the actual files' titles/statuses.

## Inputs / Secrets

Write access to the PR branch, if running in auto-commit mode.

## Outputs / Artifacts

Regenerated `adr/README.md` (auto-committed) or a diff report (check-only mode) — the choice
between the two modes is an implementation decision, not fixed by this spec.

## Failure Behavior

Blocks merge until the index matches reality, in check-only mode. In auto-commit mode, the
regenerated file is pushed and the check re-runs against the corrected state.

## Related Docs

`adr/README.md`, `adr/template.md`, `.ai/constitution.md` Article VII.
