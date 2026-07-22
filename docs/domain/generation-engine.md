# Generation Engine: Domain Rationale

> This document explains *why* Generation Engine exists in pedagogical terms. For engineering
> responsibilities, see `specs/generation-engine.md`; for the Validation-gate rationale, see ADR-005.

## The Generation Effect Is the Engine's Namesake, Not a Coincidence

The Generation Effect (`docs/pedagogy/generation-effect.md`) is the finding that people remember
material better when they generate it themselves (even partially) rather than simply reading it.
Generation Engine is named for this deliberately: its default posture is to make the *Student*
generate — an answer, an explanation, a completed step — with the Engine producing the *prompt for
that generation*, not a finished explanation to be passively consumed. When Generation Engine does
produce explanatory content (a Summary, a worked example), that's a deliberate exception made for a
specific pedagogical reason, not the default mode.

## Why Content Type Varies (MindMap, Summary, Game, Quiz, Reinforcement)

Different content types serve different encoding principles, and no single format is optimal for
every Learning Node or every Student state:

- **MindMap** externalizes relational structure — good for Dual Coding
  (`docs/pedagogy/dual-coding.md`) and for reinforcing prerequisite relationships from the Knowledge
  Graph visually.
- **Summary** supports Elaboration when it asks the Student to produce it, not just read one
  (`docs/pedagogy/elaboration.md`).
- **Quiz** is the most direct vehicle for Retrieval Practice and Active Recall
  (`docs/pedagogy/retrieval-practice.md`, `docs/pedagogy/active-recall.md`).
- **Game** exists primarily to serve "fun creates habit" (`docs/domain-principles.md` §10) —
  chosen for a Learning Node when engagement, not depth, is the limiting factor.
- **Reinforcement** is targeted specifically at low-Confidence Learning Nodes and is where Desirable
  Difficulties (`docs/pedagogy/desirable-difficulties.md`) is applied most deliberately — slightly
  harder retrieval than the Student is fully comfortable with.

Choosing among these is part of Generation Engine's decision, informed by Learning State and by
which pedagogical lever is most likely to move Confidence for that specific Learning Node — not a
random rotation.

## Why Difficulty Is a First-Class Parameter

Desirable Difficulties research (Bjork & Bjork) shows that content calibrated just beyond a
learner's current comfort produces better retention than content that's easy to process in the
moment — even though it *feels* worse to the learner while it's happening. This is precisely why
Generation Engine's target selection deliberately does not just pick the easiest available content
(`specs/generation-engine.md` R1) — comfortable content optimizes for how the Student feels *right
now*, at the expense of how much they retain.

## Why Validation Is Pedagogically Load-Bearing, Not Just a Safety Feature

ADR-005 frames Validation as a correctness/safety gate. Pedagogically, it's also what prevents a
subtle failure mode: generated content that is *plausible but pedagogically unsound* — for instance,
a question that tests memorization when the Learning Node calls for conceptual understanding, or an
explanation pitched at the wrong Bloom's Taxonomy level (`docs/pedagogy/bloom-taxonomy.md`). A
content-quality dimension of Validation checks specifically for this, distinct from factual
correctness — see `docs/prompt-engineering/prompt-review-checklist.md`.

## Why Only Problematic Learning Nodes Are Regenerated

Per `docs/domain-rules/README.md`, regeneration is targeted, not wholesale. Regenerating an entire
Session's worth of content because one Learning Node's explanation didn't land wastes Evidence
already gathered on the Learning Nodes that *did* work, and contradicts "Small Improvements Over
Perfect Lessons" (`docs/domain-principles.md` §7).

## Related Documents

`specs/generation-engine.md`, `docs/domain-model/generation-entities.md`, ADR-005,
`docs/pedagogy/generation-effect.md`, `docs/pedagogy/desirable-difficulties.md`,
`docs/prompt-engineering/generation-engine-prompt.md`.
