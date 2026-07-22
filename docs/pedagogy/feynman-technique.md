# Feynman Technique

## What It Is

Named for physicist Richard Feynman's study method, the technique asks a learner to explain a
concept in simple terms, as if teaching it to someone with no background in the subject, and to
notice specifically where the explanation breaks down or requires jargon to paper over a gap — those
breakdown points reveal exactly what isn't actually understood, as distinct from what merely feels
familiar.

## Scientific Evidence

The technique is popular pedagogy rather than a named experimental paradigm itself, but it's well
supported by two related, well-studied effects: the **self-explanation effect** (Chi et al., 1989,
1994), where learners who explain material to themselves during study — especially explaining *why*,
not just *what* — show significantly better comprehension and transfer than those who don't; and the
**protégé effect** (Fiorella & Mayer's synthesis, drawing on earlier "learning by teaching"
research), where preparing to teach or actually teaching material produces stronger learning gains
for the teacher than studying the same material for a test. Both converge on the same mechanism the
Feynman Technique exploits: generating an explanation forces retrieval, organization, and gap
detection that passive review does not.

## Why Smart App Uses It

It's one of the platform's strongest tools for **metacognitive calibration**
(`docs/pedagogy/metacognition.md`) — Students are poor at knowing what they don't know, and a failed
attempt to explain something plainly is a far more reliable signal of a real gap than a Student's
self-reported confidence.

## Where It Appears in the Product

A dedicated prompt type asking the Student to explain a Learning Node's concept in their own words,
scored not on keyword matching but on structural completeness and absence of unexplained jargon
(`docs/prompt-engineering/summary-evaluation-prompt.md` covers the closely related Summary
evaluation case).

## Engineering Implications

Evaluating a free-text explanation is a genuinely hard Validation problem (`ADR-005`) — it requires
judging conceptual completeness, not string similarity to a reference answer. This content type's
Validation logic needs its own explicit criteria (`docs/prompt-engineering/prompt-review-checklist.md`)
and its own `testing/golden-dataset.md` entries with human-reviewed example explanations at multiple
quality levels, not just a right/wrong reference answer.

## Product Implications

Feedback on a Feynman-style attempt should point at *specifically what was missing or unclear*, not
just "incorrect" — this is the whole value of the technique, and Feedback that doesn't localize the
gap wastes the exercise. This also directly serves "the child is never blamed"
(`docs/domain-rules/README.md`): the framing is "here's the part your explanation didn't cover," not
"you got this wrong."

## Prompt Implications

The evaluation prompt (`docs/prompt-engineering/prompt-review-checklist.md`) must be explicitly
instructed to identify the *specific missing or hand-waved part* of a Student's explanation and
phrase Feedback around it, resisting the much easier default of producing a generic
correct/incorrect verdict.

## Related Documents

`docs/pedagogy/elaboration.md`, `docs/pedagogy/metacognition.md`, `docs/pedagogy/generation-effect.md`,
`docs/domain-model/generation-entities.md` (Feedback).
