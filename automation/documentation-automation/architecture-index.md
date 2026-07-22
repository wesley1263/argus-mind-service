# Workflow: Architecture Index

- **Category:** Documentation Automation
- **Trigger:** `push:main` (when `.ai/architecture.md`, any `adr/ADR-*.md`, or any `specs/*.md`
  changes)
- **Blocking:** N/A (generates a derived view; the source documents it draws from are already
  gated by their own checks)

## Purpose

`.ai/architecture.md` §2's ownership table states what each Engine owns/publishes, but doesn't (and
shouldn't — it would go stale immediately) list every ADR and spec associated with each Engine. This
workflow generates that cross-reference automatically: a single "everything about Knowledge Engine"
view assembled from documents that are each independently maintained.

## Steps

1. For each Engine (and Platform, and Cross-Engine) in `.ai/architecture.md` §2's table, scan
   `adr/*.md` for ADRs whose content references that Engine by name, and `specs/*.md` for specs
   whose `Owning Engine(s):` field names it.
2. Assemble a generated page (e.g. `docs/architecture-index.md`, generated-content clearly marked as
   such at the top so no one hand-edits it) listing, per Engine: its ownership table row, every
   associated ADR (with status), every associated spec (with status), and — once `src/` exists —
   its actual package path.
3. Regenerate on every relevant merge to `main`; this is a generated artifact, not something checked
   for drift against itself the way `adr-index.md`/`glossary-index.md`/`specification-index.md`
   check hand-maintained indexes — there's no "hand-maintained version" of this page to drift from.

## Gate / Threshold

N/A — purely generative.

## Inputs / Secrets

Write access to commit the regenerated page to `main`.

## Outputs / Artifacts

A generated, always-current "architecture at a glance" page, one section per Engine.

## Failure Behavior

A generation failure is logged; the previously-generated page stays in place rather than being
replaced with a broken or partial one.

## Related Docs

`.ai/architecture.md`, `adr/README.md`, `specs/README.md`,
`automation/documentation-automation/repository-dashboard.md` (the metrics-focused counterpart —
this workflow is structural/reference, that one is health/trend).
