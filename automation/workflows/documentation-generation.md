# Workflow: Documentation Generation

- **Category:** Automation
- **Trigger:** `push:main` (when relevant source paths change)
- **Blocking:** N/A

## Purpose

Generates documentation that should always be derived from source rather than hand-maintained —
starting with the OpenAPI-derived API reference (`skills/fastapi.md`) once `src/` exists, so the
published API documentation can never silently drift from the actual FastAPI route definitions the
way hand-written docs can.

## Steps

1. On a change under any Engine's `adapters/http/` path, regenerate the OpenAPI schema by
   introspecting the FastAPI app (`skills/fastapi.md`).
2. Render the schema into human-readable API reference pages.
3. Publish the rendered pages to the project's documentation site/location (destination TBD — a
   decision for whoever sets this up, tracked via `templates/decision.md` if it's a simple hosting
   choice, or an ADR if it commits to a specific docs platform with migration cost).
4. Diff the newly generated OpenAPI schema against the previous one and hand that diff to
   `github-actions/versioning.md`'s contract-change classification step, so a schema change and a
   labeled version bump stay consistent.

## Gate / Threshold

N/A — generation, not validation. (Validation that the schema matches specs' API sections is
`github-actions/spec-drift-check.md`'s job.)

## Inputs / Secrets

Publish credentials for wherever generated docs are hosted.

## Outputs / Artifacts

Generated API reference pages; OpenAPI schema diff artifact.

## Failure Behavior

A generation failure blocks publishing the new docs (stale docs stay live rather than being
replaced with broken ones) and alerts, but does not block the merge that triggered it — this is a
downstream publishing step, not a PR gate.

## Related Docs

`skills/fastapi.md`, `automation/github-actions/spec-drift-check.md`,
`automation/github-actions/versioning.md`, `automation/documentation-automation/README.md` (the
index-generation counterpart to this — that folder regenerates *internal* index pages like the ADR
and glossary indexes; this workflow generates the *external* API reference).
