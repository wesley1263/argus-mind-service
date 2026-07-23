# ADR-002: Chunk Is an Internal-Only Concept of the Ingestion Engine

- **Status:** Accepted
- **Date:** 2026-07-21
- **Deciders:** Founding engineering (Phase 1 — Foundation)

## Context

To structure a Document into Chapters and Topics, the Ingestion Engine needs to break source text
into small pieces for its own processing — for parsing, for embedding, for whatever extraction
technique it uses internally. The exact size, shape, and boundaries of those pieces are an
implementation detail of *how* Ingestion Engine currently does its job, and that detail is expected
to keep changing as ingestion technique matures (different chunking heuristics, different
embedding models with different context windows, and so on).

If this concept were exposed to the rest of the system — in the contract Ingestion Engine
publishes, in the Knowledge Graph's provenance, in any API — then every future change to chunking
strategy would risk becoming a breaking change for Knowledge Engine or anything else downstream,
which would quietly discourage improving ingestion technique over time.

## Decision

**Chunk** is a private concept fully owned by the Ingestion Engine. No other Engine, no published
contract, no API, and no Student-facing surface may reference a Chunk. The Ingestion Engine's
public contract to the Knowledge Engine is expressed only in terms of **Document**, **Chapter**,
and **Topic** — see `memory/glossary.md`. This is reflected in `CLAUDE.md (Architecture section)` §2, where
Chunk appears only in Ingestion Engine's "Owns (private)" column, never in its "Publishes" column.

## Consequences

- **Ingestion Engine can change its chunking strategy freely** — different sizes, different
  splitting heuristics, a different embedding model — without that change ever being visible to, or
  breaking, any downstream Engine.
- **The ubiquitous language stays clean for business stakeholders:** nobody outside Ingestion Engine
  ever needs to reason about chunk boundaries to understand the platform's domain model.
- **New obligation:** Ingestion Engine must do enough synthesis internally that Topic-level output
  is genuinely useful to Knowledge Engine on its own — Knowledge Engine cannot lean on chunk-level
  granularity it isn't given, so Ingestion Engine's Topic boundaries need to be well-formed, not an
  afterthought.
- **Traceability trade-off, accepted for now:** if a future requirement needs to explain *exactly*
  which piece of source text a given Learning Node was extracted from, that traceability can't be
  expressed in terms of Chunk today. That's an explicit trade-off, not an oversight — see
  Alternatives below.

## Alternatives Considered

- **Expose Chunk-level provenance in the Knowledge Graph** (e.g., tag each Learning Node with the
  Chunk(s) it was derived from), for fine-grained traceability. Rejected for v1: it couples
  Knowledge Engine's model to Ingestion Engine's current internal representation, undermining the
  whole point of this boundary. If fine-grained provenance becomes a hard requirement, the right
  fix is a dedicated, deliberately designed provenance concept published by Ingestion Engine at the
  Topic (or a new, explicitly public) level — not leaking Chunk itself. That would be its own
  future ADR.
