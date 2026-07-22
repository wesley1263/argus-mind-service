# Evidence Engine: Domain Rationale

> This document explains *why* Evidence Engine exists in pedagogical terms. For engineering
> responsibilities, see `specs/evidence-engine.md`; for the immutability rule's engineering
> rationale, see ADR-003.

## Formative Assessment, Not Summative Testing

Evidence Engine implements what educational assessment theory calls **formative assessment**:
observation gathered *during* learning specifically to inform what happens next, as opposed to
**summative assessment**, which measures what was learned after the fact for grading or
certification. This distinction has a direct product consequence: Evidence is never scored
pass/fail against a Student — it's simply recorded as an observation, and its only job is to feed
Learning State's estimate (`docs/domain/learning-state-engine.md`). There is no concept of a
"failing grade" anywhere in this Engine, on purpose — see `docs/domain-rules/README.md`'s
"the child is never blamed."

## Why Evidence Must Be Broader Than "Correct or Incorrect"

A Student's response carries far more signal than a binary correctness judgment: response time,
whether a hint was requested, how a Feynman-style self-explanation was phrased, whether an answer
was changed before submitting. `EvidenceType` (`docs/domain-model/evidence-entities.md`) exists to
capture this variety deliberately, because Learning State's estimate is more reliable when it draws
on multiple kinds of signal rather than a single correct/incorrect bit — this mirrors how formative
assessment research treats "evidence of learning" as multidimensional, not a single test score.

## Why Immutability Is a Pedagogical Requirement, Not Just an Engineering One

ADR-003 frames Evidence immutability as an auditability and safe-iteration property. Pedagogically,
it's also a fairness property: a Student's demonstrated understanding at a point in time is a fact
about what happened, and revising history would let the system quietly "forget" real evidence of
either struggle or success. "Evidence Accumulates Forever" (`docs/domain-rules/README.md`) exists so
that a Learning Node's Confidence trajectory is always explainable by pointing at real, unaltered
observations — which is also what makes it possible to be honest with a Student (or a parent,
`docs/evaluation/README.md`'s Parent Satisfaction metric) about *why* the system believes what it
believes.

## Sessions as the Unit of Observation

A Session (`specs/evidence-engine.md`) is the natural unit within which Evidence is grouped because
it corresponds to one bounded period of a Student's attention and effort — this matters for
Cognitive Load Theory-informed pacing (`docs/pedagogy/cognitive-load-theory.md`) and for computing
engagement metrics like Session Completion (`docs/evaluation/README.md`) meaningfully. Evidence
recorded outside any Session (if that ever became a requirement) would lose this context and
shouldn't be treated as equivalent.

## Why Evidence Capture Never Blocks on Downstream Engines

`specs/evidence-engine.md` R5 (no synchronous dependency on Learning State or Generation Engine) has
a pedagogical rationale beyond reliability: a Student's act of engaging — attempting an answer,
articulating an explanation — has value the moment it happens, independent of how quickly the system
can process it. Losing an observation because a downstream computation was briefly unavailable would
be a real pedagogical loss, not just a technical inconvenience.

## Related Documents

`specs/evidence-engine.md`, `docs/domain-model/evidence-entities.md`, ADR-003,
`docs/pedagogy/retrieval-practice.md`, `docs/domain-rules/README.md`.
