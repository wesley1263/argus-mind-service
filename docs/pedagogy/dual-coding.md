# Dual Coding

## What It Is

Dual Coding Theory (Paivio, 1971, 1986) proposes that verbal and visual/nonverbal information are
processed through partially independent channels in memory, and that information encoded through
*both* channels — a concept explained in words **and** represented visually — is retained better
than information encoded through either channel alone, because it creates two independent retrieval
paths rather than one.

## Scientific Evidence

Paivio's original word/imagery recall experiments established the core effect. It was later
substantially extended by Mayer's Cognitive Theory of Multimedia Learning, which combines Dual
Coding with Cognitive Load Theory (`docs/pedagogy/cognitive-load-theory.md`) and specifies
conditions under which combining words and pictures helps (they must be presented in a coordinated,
non-redundant way — e.g., spatially and temporally contiguous) versus when it can backfire (verbatim
narration of on-screen text adds extraneous load rather than a second encoding channel). The
qualifier matters: dual coding is not "add a picture to anything," it's "represent the same
structure through two genuinely complementary channels."

## Why Smart App Uses It

MindMap content exists specifically to exploit dual coding — the same Knowledge Graph relationships
a Summary would describe in prose are instead (or additionally) represented spatially, giving the
Student a second, independent encoding of the same structure.

## Where It Appears in the Product

MindMap generation directly (`docs/domain-model/generation-entities.md`); any content type that
pairs a diagram or spatial layout with text explanation, where the two are designed to be
complementary rather than one simply restating the other.

## Engineering Implications

A MindMap's generated structure must actually correspond to real Knowledge Edges
(`specs/knowledge-engine.md`), not be a freeform, disconnected diagram — its pedagogical value comes
specifically from encoding the same relational structure the Knowledge Graph already models, giving
the Student a visual-channel version of real prerequisite/relationship data, not decoration.

## Product Implications

Visual content types should never be purely decorative additions to text content — per the "must be
complementary, not redundant" qualifier above, a MindMap that just re-lists the same sentences from
a Summary in boxes provides no dual-coding benefit and adds extraneous load instead
(`docs/pedagogy/cognitive-load-theory.md`).

## Prompt Implications

`docs/prompt-engineering/mindmap-validation-prompt.md` should explicitly check that a generated
MindMap's structure maps to real Knowledge Edges and adds spatial/relational information not already
fully captured by accompanying text — a MindMap failing this check should be treated as a
Validation failure (ADR-005), not a stylistic nitpick.

## Related Documents

`docs/pedagogy/cognitive-load-theory.md`, `docs/domain-model/generation-entities.md` (MindMap),
`docs/prompt-engineering/mindmap-validation-prompt.md`.
