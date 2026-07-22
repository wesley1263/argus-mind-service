# Priming

## What It Is

In this platform, "priming" refers specifically to **advance exposure to a concept's vocabulary and
structure before formal instruction** — closely related to Ausubel's concept of the *advance
organizer*: a piece of introductory material, presented before new content, that provides a
cognitive scaffold the learner can attach subsequent detail to. This is distinct from the narrower
sense of "priming" in cognitive psychology (implicit activation of related concepts by a preceding
stimulus, as in lexical priming experiments) — Smart App uses the term in its educational-design
sense, and this document uses it that way throughout.

## Scientific Evidence

Ausubel's advance organizer research (1960s onward) found that learners given a short, general
introduction to unfamiliar material before detailed instruction retained and transferred that
material better than learners who received the detail cold. Later meta-analytic work (e.g., Mayer's
reviews of advance organizers in the 1970s–1980s) found the effect is real but moderated by how
unfamiliar the material is to the learner — advance organizers help most when the learner has little
existing structure to attach new information to, which is precisely the situation a Student is in
*before* a class they haven't attended yet.

## Why Smart App Uses It

This is the mechanism behind the Core Product Thesis itself (`docs/domain-principles.md`): the
claim that pre-class exposure improves learning efficiency is, in effect, a claim that priming
works. If advance organizer research didn't show a benefit to structured pre-exposure, the entire
product premise would be unsupported — this principle is not one technique among many, it's the
platform's foundational bet.

## Where It Appears in the Product

Every Study Session a Student takes *before* attending the corresponding class is, functionally, a
priming intervention. Concretely: MindMap content type (`docs/domain-model/generation-entities.md`)
is a natural advance-organizer format — it presents relational structure without requiring full
mastery of every node in advance.

## Engineering Implications

Generation Engine's target selection (`specs/generation-engine.md` R1) should be capable of
producing lighter-weight, structural content (a MindMap, a short Summary) specifically for
Learning Nodes the Student hasn't encountered at all yet — distinct from the retrieval-heavy content
appropriate for a Learning Node already partially known. This is a content-type decision informed
by Confidence being near-zero/unobserved, not just low.

## Product Implications

Product surfaces should make "study before your next class" a legible, encouraged pattern — e.g.,
Session scheduling or reminders tied to a Student's actual class calendar (`docs/future/README.md`),
not just generic daily-streak mechanics.

## Prompt Implications

A priming-stage prompt (`docs/prompt-engineering/generation-engine-prompt.md`) should be instructed
to introduce structure and vocabulary without assuming prior familiarity, and explicitly avoid
assuming the Student has already seen related terms from class — the opposite instruction from a
Reinforcement-stage prompt, which can assume prior exposure.

## Related Documents

`docs/domain-principles.md` §1, `docs/domain/student-journey.md`,
`docs/pedagogy/cognitive-load-theory.md` (advance organizers reduce extraneous load by providing
structure in advance).
