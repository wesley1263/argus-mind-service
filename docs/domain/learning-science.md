# Learning Science: Foundations

This document is the entry point to `docs/pedagogy/` — it explains *why* the platform draws on
cognitive and learning science at all, how the fifteen principles in that folder relate to each
other, and how to read scientific evidence honestly rather than as marketing copy for a feature.

## Why Engineering a Learning Product Requires Learning Science

Most software products can be built well by reasoning about users' stated preferences. A learning
product cannot: what a Student *wants* in the moment (an easier question, a shorter session, a
direct answer) is frequently in tension with what actually produces durable learning. Cognitive
psychology's decades of controlled research on memory, attention, and skill acquisition are the
closest thing available to ground truth about what works — often *because* it identifies techniques
that feel harder or less pleasant in the moment (Desirable Difficulties,
`docs/pedagogy/desirable-difficulties.md`) but produce better long-term retention.

## The Two Families of Principles This Platform Draws On

**Encoding principles** — how information gets into durable memory in the first place: Dual Coding,
Elaboration, Chunking, Motor Encoding, Generation Effect. These principles mostly inform how content
is *generated and presented* (Generation Engine).

**Retrieval and consolidation principles** — how existing memory is strengthened, stabilized, and
made resistant to forgetting: Active Recall, Retrieval Practice, Spaced Repetition, Desirable
Difficulties. These principles mostly inform *when and how content is scheduled* (Learning State
Engine, Generation Engine's target selection).

Two further principles sit above both families: **Cognitive Load Theory** governs how much any
single piece of content should ask of working memory regardless of which technique produced it, and
**Metacognition** governs the Student's own awareness of what they know — which the platform both
measures (via Evidence) and tries to cultivate (via Feynman-technique-style self-explanation
prompts).

**Bloom's Taxonomy** and the **Pareto Principle** are used differently from the above: Bloom's
Taxonomy is a classification scheme (what *kind* of cognitive demand a piece of content makes),
used to keep Generation Engine's content variety honest rather than stuck at recall-level questions.
The Pareto Principle is a general heuristic, not a cognitive-science finding — see
`docs/pedagogy/pareto-principle.md` for an honest treatment of that distinction — used for
prioritizing which Learning Nodes matter most within a Chapter.

## How to Read "Scientific Evidence" in This Folder

Every document in `docs/pedagogy/` states the strength and nature of the evidence behind its
principle, deliberately distinguishing:

- **Well-replicated experimental findings** (e.g., the testing effect behind Active Recall,
  spacing effects) — these are treated as close to settled and are used with confidence.
- **Theoretical frameworks with strong but more indirect support** (e.g., Cognitive Load Theory) —
  used as a design lens, not as a source of precise numeric thresholds.
- **Heuristics borrowed from outside cognitive science** (the Pareto Principle) — used explicitly as
  a heuristic, never presented as if it were an experimental finding about memory.

This matters because `docs/prompt-engineering/` and Generation Engine's implementation will cite
these principles to justify design choices — citing evidence honestly, including its limits, is
part of respecting the Core Product Thesis (`docs/domain-principles.md`), not a hedge against
criticism.

## Where Learning Science Meets Engineering

Every `docs/pedagogy/*.md` document ends with three implication sections — Engineering, Product,
Prompt — that translate the science into concrete decisions already reflected in this repository's
specs and skills. Where a principle implies a change not yet reflected in `specs/`, that's flagged
as a gap for a future spec, not silently implemented as an assumption.

## Related Documents

`docs/pedagogy/` (all fifteen principle documents), `docs/domain-principles.md`,
`docs/prompt-engineering/README.md`.
