# Workflow: Specification Index

- **Category:** Documentation Automation
- **Trigger:** `pull_request` (when any file under `specs/` changes)
- **Blocking:** Required check

## Purpose

Keeps `specs/README.md`'s example-specifications table current — including each spec's `Status`
field, which changes over a spec's life (Draft → Approved → In Progress → Implemented) and is easy
to update in the spec itself while forgetting the index reflects the old value.

## Steps

1. Scan `specs/` for every spec file (excluding `template.md` and `README.md`).
2. Parse each spec's title and `Status:`/`Owning Engine(s):` fields.
3. Regenerate `specs/README.md`'s table from that scan; diff against committed content.
4. Additionally cross-reference against `github-actions/spec-drift-check.md`'s output: a spec marked
   `Implemented` here with open drift findings gets a visible marker in the regenerated table
   (e.g. a ⚠ column), so the index itself surfaces drift rather than requiring a separate lookup.

## Gate / Threshold

Zero drift between the index table and each spec's actual title/status/owning-Engine fields.

## Inputs / Secrets

Read access to `github-actions/spec-drift-check.md`'s latest output for the drift-marker step.

## Outputs / Artifacts

Regenerated `specs/README.md` table, including drift markers.

## Failure Behavior

Blocks merge of the spec change until the index is corrected.

## Related Docs

`specs/README.md`, `specs/template.md`, `automation/github-actions/spec-drift-check.md`.
