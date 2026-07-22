# Motor Encoding

## What It Is

Motor encoding refers to memory benefits from physically performing an action associated with
information, rather than only processing it verbally or visually — writing something by hand,
manipulating an object, tracing a shape, dragging an element into place.

## Scientific Evidence

The **enactment effect** (Engelkamp & Zimmer, and a substantial literature since the 1980s) shows
that action phrases physically performed by a learner ("subject-performed tasks") are recalled
better than the same phrases only heard or read. Separately, Mueller & Oppenheimer (2014) found
longhand note-taking produced better conceptual understanding than typing, attributed to handwriting
encouraging more selective, generative processing rather than verbatim transcription — though later
work (e.g., subsequent replications and re-analyses) suggests the effect depends substantially on
*how* notes are taken (summarizing vs. transcribing) rather than the physical modality alone, and
this document treats the finding as suggestive rather than settled.

**Honest limitation**: motor encoding is the pedagogy principle with the weakest direct applicability
to a software product — a touchscreen drag gesture or keyboard interaction is not equivalent to
handwriting or full-body enactment, and this document does not claim it is. What follows is a
deliberately modest application, not an overclaim.

## Why Smart App Uses It

Where a content type naturally involves manipulation (arranging MindMap nodes, dragging items into a
sequence or category), the interaction is preserved as an actual physical manipulation rather than
collapsed into a passive multiple-choice equivalent, on the reasoning that *some* motor involvement
plausibly contributes a small additional encoding benefit beyond the retrieval/generation benefit
alone — consistent with, though not equivalent to, the enactment-effect literature.

## Where It Appears in the Product

MindMap construction/editing (dragging nodes and connections); any Game content type involving
physical manipulation (sorting, matching, sequencing) rather than pure tap-to-select.

## Engineering Implications

Interaction design for these content types should preserve manipulation as an actual gesture
(drag, draw, arrange) captured as its own `EvidenceType`
(`docs/domain-model/evidence-entities.md`) — not simplified into an equivalent "select from a list"
interaction purely for implementation convenience, since that would discard the one dimension this
principle actually predicts matters.

## Product Implications

This principle should not be used to justify adding gratuitous physical interaction to formats that
don't naturally call for it — given the weak and qualified evidence base, it's a tiebreaker between
otherwise-equivalent interaction designs, not a mandate to gamify every content type.

## Prompt Implications

Where Generation Engine produces a manipulable content type (e.g., a MindMap or sorting Game), the
prompt should specify structure suited to physical arrangement (a small number of clearly
distinguishable elements) rather than structure that only makes sense as static text.

## Related Documents

`docs/pedagogy/generation-effect.md`, `docs/domain-model/generation-entities.md` (MindMap, Game),
`docs/domain-model/evidence-entities.md`.
