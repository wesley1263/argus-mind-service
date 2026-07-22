# Chunking

> **Naming collision, disambiguated explicitly**: this document describes the cognitive-science
> principle of chunking. It is **not** the same concept as **Chunk**, Ingestion Engine's internal,
> implementation-only unit of text (`glossary/README.md`, ADR-002). The two share a name by
> coincidence of English vocabulary, not by design — Ingestion Engine's Chunk boundaries are drawn
> for computational convenience (fitting an extraction/embedding technique), while cognitive
> chunking, described below, is about how information is grouped for *human* working-memory
> capacity. They are related only in spirit (`docs/domain/knowledge-engine.md` notes the
> resonance), never in implementation — Chunk must never be treated as a proxy for cognitively
> appropriate content grouping.

## What It Is

Chunking is the cognitive process of grouping individual pieces of information into larger,
meaningful units, so that working memory — which holds a small number of *items* regardless of each
item's internal complexity — can effectively hold more total information by treating each group as
a single item.

## Scientific Evidence

Miller's (1956) "The Magical Number Seven, Plus or Minus Two" established working memory's limited
item capacity and proposed chunking as the mechanism that lets experience effectively expand it.
Chase & Simon's (1973) chess studies gave this a concrete demonstration: chess masters could
reconstruct realistic mid-game board positions far better than novices, not because of superior raw
memory, but because years of experience let them perceive the board as meaningful chunks (known
piece configurations) rather than individual piece placements — directly supporting
`docs/domain/knowledge-engine.md`'s claim that expertise is structural, not just volumetric.

## Why Smart App Uses It

This is the direct justification for extracting Learning Nodes at Topic-level granularity
(`specs/document-ingestion.md`, `specs/knowledge-engine.md`) rather than sentence-level or
Chapter-level: a Learning Node should correspond to a chunk-sized, meaningfully bounded unit a
Student can hold as one item in working memory, not an arbitrary slice of text.

## Where It Appears in the Product

Learning Node granularity itself; SessionPlan pacing (introducing a bounded number of new Learning
Nodes per Session, per Cognitive Load Theory — `docs/pedagogy/cognitive-load-theory.md`); MindMap
grouping of related nodes into visual clusters.

## Engineering Implications

Knowledge Engine's extraction quality (`testing/golden-dataset.md`) should be evaluated partly on
whether extracted Learning Nodes correspond to real conceptual chunks recognizable to a subject-matter
reviewer — not merely on structural well-formedness (no cycles, valid edges). A structurally valid
but poorly-chunked graph (too fine-grained, too coarse) still fails this principle even though it
passes `testing/architecture-testing.md`'s checks.

## Product Implications

Product content-length limits per generated item should be set with working-memory-sized chunks in
mind — a Quiz question or Summary section should cover one chunk-sized idea, not several concepts
compressed into one item because it was efficient to generate that way.

## Prompt Implications

Extraction and generation prompts (`docs/prompt-engineering/knowledge-engine-prompt.md`) should be
explicitly instructed to prefer a Learning Node boundary that corresponds to "one thing a Student
would recognize as one idea," using worked examples from `docs/golden-dataset/structure.md` as
calibration, rather than a purely statistical or length-based splitting heuristic.

## Related Documents

`docs/domain/knowledge-engine.md`, `docs/pedagogy/cognitive-load-theory.md`, ADR-002,
`glossary/README.md` (Chunk).
