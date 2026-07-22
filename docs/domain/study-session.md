# Study Session: Domain Rationale

> **Naming note:** "Study Session" is the product/pedagogical name for what `glossary/README.md`
> defines as **Session** — the bounded period of Student interaction Evidence Engine captures. This
> document (and `docs/domain-model/evidence-entities.md`) treats the two as the same entity viewed
> from different angles: "Session" is the engineering/data term, "Study Session" is how it's talked
> about in product and pedagogy contexts. There is deliberately no separate persisted entity — see
> `specs/study-session.md`'s own note on this, which established the same resolution at the
> engineering-spec level.

## Why a Session Is the Right Unit of Pedagogical Design

Cognitive Load Theory (`docs/pedagogy/cognitive-load-theory.md`) and attention research both point
to the same practical constraint: working memory and sustained attention are limited resources that
deplete over the course of continuous engagement. A Study Session is the unit within which the
platform manages that budget deliberately — pacing content, varying format, and deciding when a
Student has done enough for one sitting, rather than presenting an undifferentiated stream of content
with no structure.

## What a Well-Designed Study Session Looks Like

1. **Opens with something the Student can succeed at** — typically a low-difficulty retrieval prompt
   on an already-reasonably-confident Learning Node, to build momentum before Desirable Difficulties
   is introduced.
2. **Ramps into Desirable Difficulty** — the core of the Session targets Learning Nodes just past
   the Student's current Confidence (`docs/domain/adaptive-learning.md`).
3. **Varies format** — per Cognitive Load Theory and Dual Coding, alternating content types (Quiz,
   MindMap, Summary) prevents any single format's extraneous load from dominating the Session.
4. **Ends deliberately, not abruptly** — a Session that simply runs until the Student quits misses
   the chance to end on a note that supports Voluntary Return Rate (`docs/evaluation/README.md`).

This shape is what a **SessionPlan** (`docs/domain-model/generation-entities.md`) encodes before a
Study Session begins — the plan is Generation Engine's answer to "given this Student's Learning
State right now, what should the next N pieces of content be and in what order," informed by the
structure above, not a single content request repeated blindly.

## Why Sessions Are Bounded, Not Continuous

An unbounded stream of content has no natural point to apply spacing, no natural point to vary
difficulty deliberately, and no clear unit for measuring engagement (`docs/evaluation/README.md`'s
Session Completion metric requires a Session to actually complete). Bounding a Session is what makes
Spaced Repetition across Sessions (`docs/pedagogy/spaced-repetition.md`) a meaningful concept in the
first place — spacing is defined in terms of the gaps *between* Sessions.

## Session Length as a Product Decision, Not a Technical Default

Session length should be short enough to sustain habit (`docs/domain-principles.md` §10–11) and long
enough to make meaningful progress on at least one Learning Node. This is a genuine product tuning
question, informed by `docs/evaluation/README.md`'s Session Completion and Voluntary Return Rate data
— not fixed by this document, which states the pedagogical constraints the eventual number must
satisfy, not the number itself.

## Related Documents

`specs/study-session.md`, `specs/evidence-engine.md`, `docs/domain-model/evidence-entities.md`,
`docs/domain/adaptive-learning.md`, `docs/pedagogy/cognitive-load-theory.md`,
`docs/pedagogy/spaced-repetition.md`.
