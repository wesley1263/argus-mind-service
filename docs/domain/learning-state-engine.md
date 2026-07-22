# Learning State Engine: Domain Rationale

> This document explains *why* Learning State Engine exists in pedagogical terms. For engineering
> responsibilities, see `specs/learning-state-engine.md`; for the derived-projection rule's
> engineering rationale, see ADR-004.

## Mastery Learning, Made Explicit

Learning State Engine operationalizes **mastery learning** theory (Bloom, 1968; Carroll's model of
school learning): the idea that most Students can reach a comparable level of understanding given
the right amount of time and the right instructional approach targeted to where they currently are
— as opposed to a fixed pace where some Students inevitably fall behind. Confidence per Learning
Node is the platform's operational stand-in for "where a Student currently is," used to target
instruction, not to rank or sort Students against each other.

## Why Confidence Is Never Binary

A Student rarely "knows" or "doesn't know" a concept in a clean binary sense — understanding exists
on a continuum, and a single correct answer doesn't rule out lucky guessing any more than a single
mistake rules out real understanding. Modeling Confidence as a continuous, probabilistic estimate
(`docs/domain-rules/README.md` — "Confidence is never binary") reflects this directly, and is closer
in spirit to Bayesian Knowledge Tracing approaches used in the intelligent tutoring systems
literature than to a simple pass/fail gate.

## Why Confidence Decays

Forgetting is not a bug in a Student's memory to be "fixed" — it's the expected, well-documented
behavior described by the forgetting curve (Ebbinghaus) and is the entire reason Spaced Repetition
(`docs/pedagogy/spaced-repetition.md`) works as a technique at all: revisiting material just as it
would otherwise be forgotten produces stronger retention than revisiting it too early or not at all.
Confidence's decay-over-time behavior (`specs/learning-state-engine.md` R3) exists so the platform's
model of a Student stays honest about this, rather than treating a Learning Node touched once, months
ago, as still solid today.

## False Negatives vs. False Positives

`docs/domain-rules/README.md` states that false negatives (treating a Student as not understanding
something they actually do) are worse than false positives (treating a Student as understanding
something they don't, briefly). The reasoning: a false positive gets corrected quickly — the next
piece of content targeting that "mastered" node will surface the gap and Evidence will correct
Confidence downward. A false negative wastes a Student's time revisiting something they've already
learned, which directly undermines engagement (`docs/domain-principles.md` §10–11, "fun creates
habit") and contradicts efficiency being the entire point of the Core Product Thesis. This asymmetry
is a real, load-bearing design constraint on the confidence model, not an incidental preference.

## Why the Model Version Is Recorded (Beyond Auditability)

Pedagogically, being able to say "this Student's Confidence trajectory was computed under model
version 3, which weighted self-explanation Evidence more heavily than v2" is what makes it possible
to evaluate whether a *change to the pedagogy itself* helped or hurt — this is how
`docs/evaluation/README.md`'s Learning Node Stability and Confidence Growth metrics stay meaningful
across model iterations rather than becoming incomparable noise.

## Related Documents

`specs/learning-state-engine.md`, `docs/domain-model/learning-state-entities.md`, ADR-004,
`docs/pedagogy/spaced-repetition.md`, `docs/domain-rules/README.md`.
