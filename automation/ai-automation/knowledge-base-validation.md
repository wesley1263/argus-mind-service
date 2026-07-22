# Workflow: Knowledge Base Validation

- **Category:** AI Automation
- **Trigger:** `schedule` (nightly), and on-demand after any bulk Document ingestion
  (`specs/document-ingestion.md`)
- **Blocking:** Advisory, except the structural integrity check below

## Purpose

`specs/knowledge-engine.md`'s Acceptance Criteria and Risks table are checked at write time
(per-Document, as it's ingested); this workflow validates the **accumulated** Knowledge Graph as a
whole, on an ongoing basis, catching problems that only become visible once many Documents'
extractions interact — cross-Document near-duplicate Learning Nodes, an edge structure that's
locally valid per-Document but forms an unexpected global pattern, or slow quality drift in
extraction that no single ingestion's checks would flag.

## Steps

1. **Structural integrity** (mirrors `testing/architecture-testing.md`'s per-write check, re-run
   against the live graph as a whole): confirm zero prerequisite cycles exist anywhere in the full
   Knowledge Graph, not just within a single Document's newly-added edges.
2. **Orphan detection**: flag Learning Nodes with no Knowledge Edges at all — likely either a
   genuinely foundational concept (expected, fine) or a mis-extraction (worth review); the
   distinction is a judgment call surfaced for human triage, not auto-resolved.
3. **Cross-Document overlap**: flag Learning Nodes across different Documents with high textual/
   semantic similarity — candidates for the de-duplication/merging capability
   `specs/knowledge-engine.md` explicitly deferred to Future Work; until that capability exists,
   this step only reports candidates, it doesn't merge anything.
4. **Extraction quality trend**: re-score a sample of the live graph against
   `testing/golden-dataset.md`'s Knowledge Engine dataset criteria to catch slow drift, the same
   principle as `prompt-regression.md` applied to extraction rather than generation.

## Gate / Threshold

**Hard block** (flagged as a data-integrity incident, not just a report) on any detected
prerequisite cycle — this violates `specs/knowledge-engine.md`'s own Acceptance Criteria and should
never occur if per-write validation is working; its appearance in the live graph means that
validation itself has a gap. Orphan and cross-Document overlap findings are advisory triage queues.

## Inputs / Secrets

Read access to the Knowledge Graph store (`skills/postgres.md`'s `knowledge` schema, via Knowledge
Engine's own published contract per `.ai/architecture.md` §4 — this workflow does not query the
schema directly, even though it's "just" a monitoring job).

## Outputs / Artifacts

Structural integrity report (should always be clean); orphan and overlap triage queues; extraction
quality trend, feeding `workflows/repository-health.md`.

## Failure Behavior

A detected cycle pages the Knowledge Engine owner as a data-integrity incident and triggers an
immediate audit of recent ingestions to find which one introduced it, since per-write validation
(`specs/knowledge-engine.md`) should have caught it before it was ever persisted.

## Related Docs

`specs/knowledge-engine.md`, `testing/architecture-testing.md`, `testing/golden-dataset.md`,
`.ai/architecture.md` §4.
