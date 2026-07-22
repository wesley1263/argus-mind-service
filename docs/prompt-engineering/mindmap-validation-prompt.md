# Prompt Design Guide: MindMap Validation

**Job**: check a generated MindMap against `adaptive-content-prompt.md`'s base contract plus the
specific requirement that makes a MindMap pedagogically valuable at all — real structural
correspondence to the Knowledge Graph (`docs/pedagogy/dual-coding.md`).

## Required Context

The generated MindMap's proposed nodes and connections; the actual Knowledge Edges for the target
Learning Node's neighborhood, fetched from Knowledge Engine's contract (never re-derived by the
validating prompt itself from general knowledge).

## Checks This Prompt Must Perform

1. **Structural correspondence**: does every connection drawn in the MindMap correspond to a real
   Knowledge Edge, or a defensible generalization of one? A connection with no basis in the actual
   graph fails — it would mislead the Student about real prerequisite structure.
2. **Non-redundancy with accompanying text** (`docs/pedagogy/dual-coding.md`'s "must be
   complementary, not redundant" qualifier): does the MindMap add spatial/relational information, or
   does it just restate sentences from an accompanying Summary in boxes?
3. **Element count**: within Cognitive Load Theory bounds for a single MindMap
   (`docs/pedagogy/cognitive-load-theory.md`) — flag if node count exceeds the configured maximum
   for the target Difficulty/age band.

## Output

A structured pass/fail per check, feeding ADR-005's Validation outcome — not a single holistic
verdict, so a specific failing check (e.g., "connection C has no basis in real edges") can drive a
targeted regeneration instruction rather than a full retry from scratch.

## Related Documents

`docs/pedagogy/dual-coding.md`, `docs/domain-model/generation-entities.md` (MindMap),
`docs/prompt-engineering/adaptive-content-prompt.md`, ADR-005.
