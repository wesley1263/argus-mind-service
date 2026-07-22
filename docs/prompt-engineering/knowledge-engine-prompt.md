# Prompt Design Guide: Knowledge Engine

**Job**: turn a Topic (`docs/domain-model/ingestion-entities.md`) into candidate Learning Nodes and
Knowledge Edges (`specs/knowledge-engine.md` R1–R2). Lives in Knowledge Engine's `adapters/`, per
`skills/clean-architecture.md`.

## Required Context (PromptContext)

- The Topic's title and summary, and its Chapter's title (for document-level context) — **never**
  Chunk data (ADR-002).
- Already-extracted Learning Nodes from earlier Topics in the same Document, so the model can
  propose Knowledge Edges to genuinely prior material instead of only within the current Topic.

## Instructions the Prompt Must Encode

1. Propose Learning Nodes at **Topic-appropriate granularity** — one recognizable idea per node, not
   one per sentence and not the whole Topic as one node (`docs/pedagogy/chunking.md`).
2. For each proposed node, state a plain-language justification a subject-matter reviewer could
   check against `testing/golden-dataset.md` — this becomes the node's provenance rationale.
3. Propose `prerequisite_of` edges only where a genuine dependency exists (understanding B requires
   A), not merely "these are related" — related-but-not-dependent nodes are a distinct edge type,
   not encoded as false prerequisites.
4. Never reference or imply the existence of Chunk-level structure in the output.

## What Validation Must Check

Acyclicity of any newly proposed edges against the existing graph
(`testing/architecture-testing.md`'s structural check applies here); that every proposed node maps
to real Topic content, not fabricated material.

## Failure Mode to Design Against

A model asked to "extract concepts" defaults to over-fragmentation (near-sentence-level nodes) far
more often than under-fragmentation — the prompt should bias explicitly toward Topic-appropriate
chunking, not leave granularity to the model's own judgment.

## Related Documents

`specs/knowledge-engine.md`, `docs/domain/knowledge-engine.md`, `docs/pedagogy/chunking.md`,
`docs/golden-dataset/structure.md` (Expected Learning Nodes).
