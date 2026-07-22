# Documentation Automation

Workflows that keep the repository's own indexes and cross-references honest — the automated
enforcement of `.ai/development-principles.md` §9 ("Documentation Is Part of the Change") applied
specifically to the index/summary pages that are easy to forget when their source content changes.

## Index

| Workflow | Keeps in sync |
|---|---|
| [`adr-index.md`](adr-index.md) | `adr/README.md`'s table vs. the actual ADR files and their statuses. |
| [`glossary-index.md`](glossary-index.md) | `glossary/README.md`'s own Alphabetical Index vs. its term headings. |
| [`specification-index.md`](specification-index.md) | `specs/README.md`'s table vs. the actual spec files, statuses, and open drift findings. |
| [`architecture-index.md`](architecture-index.md) | A generated "everything about Engine X" cross-reference of ADRs and specs, assembled from documents each already independently maintained. |
| [`repository-dashboard.md`](repository-dashboard.md) | A single always-current page rolling up every metric/health workflow's output. |

## Two Different Kinds of "Keeping in Sync"

- `adr-index.md`, `glossary-index.md`, `specification-index.md` check a **hand-maintained** summary
  (a table someone could, in principle, edit by hand) against its source of truth and block on
  drift — these guarantee an existing document never lies about the files it summarizes.
- `architecture-index.md` and `repository-dashboard.md` are **purely generated** pages with no
  hand-maintained counterpart to drift from — they're regenerated wholesale on every relevant
  change and never manually edited at all.

## Relationship to `documentation-generation.md`

`automation/workflows/documentation-generation.md` generates *external*, code-derived documentation
(the OpenAPI API reference). Everything in this folder generates *internal* index/reference pages
derived from other markdown documents already in this repository. Different sources, same
principle: a page that summarizes something else is only trustworthy if it's mechanically kept
honest.
