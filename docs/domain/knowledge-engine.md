# Knowledge Engine: Domain Rationale

> This document explains *why* Knowledge Engine exists in pedagogical terms. For its engineering
> responsibilities and contract, see `specs/knowledge-engine.md`; for architecture, see
> `.ai/architecture.md` §2.

## The Cognitive Science Behind a Knowledge Graph

Expertise research (notably Chase & Simon's studies of chess players) shows that experts don't just
know more isolated facts than novices — they hold that knowledge in richly interconnected
structures, so that recognizing one element activates related ones automatically. A novice's
knowledge, by contrast, tends to be a collection of disconnected facts. Building a Knowledge Graph
of Learning Nodes and prerequisite Knowledge Edges is a direct attempt to give a Student
expert-like *structure*, not just expert-like *content* — see `docs/domain/product-philosophy.md`'s
"mental model" claim.

## Why Prerequisite Structure Specifically

Cognitive Load Theory (`docs/pedagogy/cognitive-load-theory.md`) predicts that presenting a concept
before its prerequisites are in place imposes unmanageable extraneous load — the Student has nowhere
to attach the new information. Knowledge Engine's prerequisite Knowledge Edges
(`specs/knowledge-engine.md` R2) exist specifically so Generation Engine's selection logic
(`specs/generation-engine.md` R1) never targets a Learning Node whose prerequisites aren't
sufficiently established — this is a pedagogical requirement expressed as an engineering constraint,
not the other way around.

## Why Topic-Level Extraction, Not Sentence-Level

Extracting a Learning Node per sentence would produce trivially small, disconnected units that don't
correspond to any real conceptual boundary a Student would recognize — the opposite of the "mental
model" goal. Extracting one Learning Node per entire Chapter would be too coarse to adapt to
(`docs/domain/adaptive-learning.md`'s targeted-selection requirement needs finer granularity than
that). Topic-level extraction (`specs/document-ingestion.md`) is the granularity at which real
courses tend to organize conceptual units, which is why it was chosen as the extraction boundary —
this is a pedagogical judgment call, checked against `testing/golden-dataset.md`'s Knowledge Engine
dataset, not a technical default.

## Why Chunk Stays Internal (Reinforcing ADR-002)

Ingestion Engine's Chunk exists purely to make text processable by an extraction technique — it has
no pedagogical meaning of its own and its boundaries are drawn for computational convenience (fitting
a model's context window, for instance), not for conceptual coherence. Elevating Chunk to a
business concept would imply its boundaries mean something to a Student's understanding, which they
don't. This is the pedagogical justification behind what ADR-002 already established as an
engineering rule.

## The Knowledge Graph Is Never "Finished"

Consistent with "Knowledge Always Evolves Incrementally" (`docs/domain-rules/README.md`), the
Knowledge Graph is treated as a living structure that grows as Documents are ingested — never as a
one-time curriculum design exercise. This has a direct product consequence: a Student's experience
should get *better*, not just *different*, as more material is ingested, because Learning Nodes gain
richer prerequisite context over time.

## What Knowledge Engine Deliberately Does Not Do

It does not personalize the graph per Student — the Knowledge Graph represents what *can* be known,
uniformly; personalization happens downstream, in Learning State (what *this* Student knows) and
Generation Engine's selection (what to show *this* Student next). Conflating "the map" with "the
route" would make the graph unreliable as shared ground truth across Students.

## Related Documents

`specs/knowledge-engine.md`, `docs/domain-model/knowledge-entities.md`, ADR-002,
`docs/pedagogy/cognitive-load-theory.md`, `docs/pedagogy/chunking.md` (the cognitive-science
principle, not the Ingestion Engine artifact — see that document's disambiguation note).
