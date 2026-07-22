# Domain Principles

> Status: **Permanent.** This document is the pedagogical and product counterpart to
> `/.ai/constitution.md`: where the Constitution states the non-negotiable *engineering* rules,
> this document states the non-negotiable *learning* rules. Every document under `docs/domain/`,
> `docs/pedagogy/`, `docs/domain-rules/`, and `docs/prompt-engineering/` exists to serve these
> principles. Where an engineering convenience and a principle below conflict, the principle wins —
> that conflict is exactly what `.ai/constitution.md` Article I ("The Methodology Is the Product")
> exists to protect.

## The Core Product Thesis

> A student who builds a mental model of the subject **before** classroom exposure learns more
> efficiently than a student encountering the content for the first time during class.

Every principle below is a consequence of taking this thesis seriously, not a separate list of good
ideas. If a proposed feature doesn't trace back to this thesis, it doesn't belong in Smart App —
see `docs/domain/product-philosophy.md`.

## The Principles

### 1. Learning Before Classroom

The platform's job happens *before* the Student sits in class, not as a substitute for the
classroom and not as after-the-fact remediation. Priming a Student with a mental model in advance
is a different pedagogical act than reviewing material they already sat through — see
`docs/pedagogy/priming.md`.

### 2. The Child Builds Knowledge

The platform does not hand a Student a finished explanation and call that learning. It structures
material and prompts retrieval so the Student constructs their own understanding — consistent with
constructivist learning theory and with the Generation Effect (`docs/pedagogy/generation-effect.md`).
Content that could be understood passively is a missed opportunity, not a convenience.

### 3. The Classroom Reinforces

Because the Student arrives primed, classroom time reinforces and deepens a mental model that
already exists rather than installing one from nothing. The platform is not competing with the
classroom for the Student's first exposure to a concept — it's setting up the classroom to work
better.

### 4. Evidence Over Assumptions

Nothing about a Student's understanding is assumed — it's observed. Every claim the system makes
about what a Student knows traces back to Evidence (`.ai/constitution.md` Article IV,
`docs/domain/evidence-engine.md`). A confident-sounding system that isn't actually grounded in
observation is worse than a cautious one that is.

### 5. Adaptation Over Repetition

The platform does not "cover material again" identically when a Student struggles. It adapts —
different framing, different difficulty, different modality (`docs/pedagogy/dual-coding.md`) — on
the premise that if an explanation didn't work once, repeating it unchanged is unlikely to work the
second time either.

### 6. Understanding Over Memorization

Retrieval Practice and Spaced Repetition (`docs/pedagogy/retrieval-practice.md`,
`docs/pedagogy/spaced-repetition.md`) are used in service of durable *understanding*, not rote
recall of surface facts. A Learning Node is never considered mastered because a Student memorized
one phrasing of an answer — see `docs/domain-rules/README.md`'s stability rules.

### 7. Small Improvements Over Perfect Lessons

The platform prefers many small, targeted, evidence-driven adjustments over one large, perfectly
polished lesson delivered once. This mirrors Desirable Difficulties research
(`docs/pedagogy/desirable-difficulties.md`) and the Engine architecture itself: Generation Engine
regenerates only the specific Learning Nodes that need it (`docs/domain-rules/README.md`), never a
wholesale re-teach.

### 8. Learning Is Incremental

Knowledge Graph construction, Learning State computation, and content generation all assume
knowledge accumulates in small, connected steps — never a discontinuous jump. This is why Knowledge
Engine models prerequisite structure explicitly (`docs/domain/knowledge-engine.md`) rather than
treating each Learning Node as independent.

### 9. Every Interaction Teaches

There is no "throwaway" interaction. A quiz answer, a mind map edit, a hint request — every one of
these is both an opportunity to help the Student learn *and* a source of Evidence about what they
know. Product surfaces are designed with both jobs in mind simultaneously, never just one.

### 10. Fun Creates Habit

Engagement is not a vanity feature — Games and other lightweight formats
(`docs/domain-model/generation-entities.md`) exist because a Student who doesn't return doesn't
learn, no matter how sound the underlying pedagogy is. Fun is instrumental to the thesis, not in
tension with it.

### 11. Habit Creates Mastery

Consistency compounds. A Student who returns often, even briefly, benefits more from Spaced
Repetition and incremental Knowledge Graph coverage than one who studies intensely but rarely — see
`docs/evaluation/README.md`'s Voluntary Return Rate metric.

### 12. The System Estimates Learning

Learning State and Confidence are always estimates, never certainties (`docs/domain-rules/README.md`
— "Learning State is estimated," "Confidence is never binary"). The system is designed throughout
to behave correctly under uncertainty, not to pretend the uncertainty isn't there.

### 13. The System Never Judges Learning

The platform never frames a Student's current state as a verdict on them — a low Confidence value
is information about what to do next, never a label applied to the Student
(`docs/domain-rules/README.md` — "the child is never blamed"). This principle constrains product
copy, Feedback design, and even internal naming: the system estimates *Learning Nodes*, not
*students' worth*.

## How to Use This Document

- Writing a spec (`specs/`)? Check that it serves at least one principle above explicitly, and
  doesn't quietly violate another.
- Writing a prompt (`docs/prompt-engineering/`)? Every prompt guide states which principle(s) it's
  responsible for upholding at the model-output level.
- Reviewing a PR? `.ai/review-checklist.md` covers engineering boundaries; this document is the
  complementary check for whether the *pedagogy* is still sound — a technically correct change can
  still violate principle 4, 5, or 13.

## Related Documents

`.ai/constitution.md` (the engineering counterpart), `docs/domain/product-philosophy.md` (the fuller
narrative version of the Core Product Thesis), `docs/domain-rules/README.md` (the operational rules
these principles imply).
