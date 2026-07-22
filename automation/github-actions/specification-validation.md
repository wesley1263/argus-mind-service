# Workflow: Specification Validation

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (when any file under `specs/` changes, or when an implementation PR
  claims to implement one)
- **Blocking:** Required check

## Purpose

Confirms a spec in `/specs/` is internally well-formed against `specs/template.md` before it's
treated as Approved — the automated counterpart to a human reviewer checking a spec's completeness.

## Steps

1. For every changed file under `specs/` (excluding `specs/template.md` and `specs/README.md`),
   confirm every section from `specs/template.md` is present: Business Context, Goals, Requirements,
   Acceptance Criteria, Sequence Diagram, State Diagram, API, Events, Database, Risks, Future Work,
   Definition of Done — either filled in or explicitly marked "Not applicable: <why>"
   (`specs/template.md`'s own stated convention).
2. Render every Mermaid block in the changed spec(s) to confirm valid syntax (a diagram that fails
   to render is treated the same as a missing diagram).
3. Extract every business-concept-looking term (capitalized multi-word phrases matching the
   glossary's naming pattern) used in the spec and confirm each exists in `glossary/README.md`, or
   is flagged as a likely-missing term for human confirmation.
4. Confirm the spec's declared "Owning Engine(s)" field matches an Engine (or "Platform" or
   "Cross-Engine") from `.ai/architecture.md` §2's table — not a free-text value.

## Gate / Threshold

All template sections present (or explicitly marked N/A); all Mermaid diagrams render; zero
unresolved business terms outside the glossary (this is the mechanical enforcement of
`.ai/constitution.md` Article VI at the spec-authoring stage, before any code exists to catch it
via `architecture-validation.md`).

## Inputs / Secrets

None.

## Outputs / Artifacts

Per-spec completeness report; list of any terms flagged as possibly missing from the glossary.

## Failure Behavior

Blocks merge of the spec change itself. Does not block unrelated PRs.

## Related Docs

`specs/template.md`, `glossary/README.md`, `.ai/constitution.md` Article VI,
`automation/github-actions/spec-drift-check.md` (the complementary check for specs vs. running
code, rather than specs vs. their own template).
