# Spaced Repetition

## What It Is

Spaced repetition is the scheduling of review sessions for a given piece of knowledge at
increasing intervals over time, rather than massed together (cramming). The core insight is that
revisiting information just as it's about to be forgotten produces stronger, more durable
consolidation than revisiting it while it's still fresh — and far stronger retention per unit of
study time than reviewing it repeatedly in one sitting.

## Scientific Evidence

Hermann Ebbinghaus's late-19th-century self-experiments first documented the *forgetting curve* —
memory strength decaying roughly logarithmically over time without reinforcement. The *spacing
effect* built on this: Cepeda et al.'s 2006 meta-analysis (over 250 studies) confirmed that spaced
practice outperforms massed practice across a wide range of materials and retention intervals, and
found that the optimal spacing interval scales with how long retention needs to last — a detail
directly relevant to how Learning State's decay model should be calibrated
(`docs/domain/learning-state-engine.md`). This is among the most consistently replicated findings in
learning science.

## Why Smart App Uses It

Confidence's decay-over-time behavior (`specs/learning-state-engine.md` R3) and Generation Engine's
target selection (`specs/generation-engine.md` R1) are the platform's direct implementation of the
spacing effect: a Learning Node's priority for revisiting rises specifically as its estimated
Confidence decays, mirroring the "revisit just before forgetting" principle rather than a fixed
review calendar.

## Where It Appears in the Product

Every returning Study Session implicitly applies spacing by construction — Generation Engine doesn't
need a separate "review mode," because target selection already prioritizes decayed Learning Nodes
alongside new ones in the same mechanism (`docs/domain/adaptive-learning.md`'s "same mechanism, two
timescales" point).

## Engineering Implications

The decay function in Learning State Engine (`specs/learning-state-engine.md`) should not be a fixed
half-life applied uniformly — evidence suggests spacing intervals should scale with prior retention
strength (a Learning Node reinforced many times decays more slowly than one touched only once). This
is a concrete, testable parameter to validate against `testing/golden-dataset.md`'s Learning State
scenarios, not something to hardcode arbitrarily.

## Product Implications

The product should never present "spaced review" as a separate, opt-in feature a Student has to
seek out — per the engineering point above, it's built into ordinary Study Session flow. Making it
visible as a distinct mode would undercut habit formation (`docs/domain-principles.md` §11) by
turning something automatic into extra work.

## Prompt Implications

When Generation Engine targets a decayed (previously-confident, now-fading) Learning Node, the
prompt should be instructed to treat this differently from a first encounter — referencing prior
exposure implicitly rather than re-teaching from scratch, since the Student has real prior context
to retrieve, not a blank slate (`docs/prompt-engineering/generation-engine-prompt.md`).

## Related Documents

`docs/pedagogy/active-recall.md`, `docs/pedagogy/retrieval-practice.md`,
`docs/domain/learning-state-engine.md`, `specs/learning-state-engine.md`.
