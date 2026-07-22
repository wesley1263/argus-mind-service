# Workflow: Glossary Index

- **Category:** Documentation Automation
- **Trigger:** `pull_request` (when `glossary/README.md` changes)
- **Blocking:** Required check

## Purpose

Keeps `glossary/README.md`'s own "Alphabetical Index" table (`glossary/README.md`'s closing
section) mechanically in sync with the term definitions above it — the glossary is the single most
depended-upon document in the repository (`.ai/constitution.md` Article VI), so its own internal
consistency can't rely on manual upkeep.

## Steps

1. Parse every `### <Term>` heading in `glossary/README.md`, along with which Engine-ownership
   section (`## Ingestion Engine`, etc., or `## Cross-Cutting Terms`) it falls under.
2. Regenerate the Alphabetical Index table from that scan.
3. Diff against the committed table; also confirm every term marked *(internal only)* or
   *(informal)* in a heading carries the matching annotation in the index row.
4. Cross-check: does any term appear in the index with no corresponding `###` heading (a stale
   entry), or vice versa (a new term missing from the index)?

## Gate / Threshold

Zero drift between the index and the actual term headings, including internal-only/informal
annotations matching exactly.

## Inputs / Secrets

None (or write access, if run in auto-commit mode, per `adr-index.md`'s same choice).

## Outputs / Artifacts

Regenerated index table or a diff report.

## Failure Behavior

Blocks merge of the glossary change until the index is corrected.

## Related Docs

`glossary/README.md`, `.ai/constitution.md` Article VI.
