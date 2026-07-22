# Bloom's Taxonomy

## What It Is

Bloom's Taxonomy is a classification of cognitive demand levels for learning objectives, originally
published by Benjamin Bloom and collaborators in 1956 and substantially revised by Anderson &
Krathwohl in 2001. The revised, commonly used version orders six levels from lower to higher
cognitive demand: **Remember**, **Understand**, **Apply**, **Analyze**, **Evaluate**, **Create**.
Unlike most other documents in this folder, Bloom's Taxonomy is a *classification scheme*, not a
mechanism that predicts a memory or learning effect — its evidentiary basis is its decades of
practical use and refinement in instructional design, not a controlled experimental finding.

## Scientific Evidence

The taxonomy's original hierarchical ordering (that each level strictly requires mastery of the one
below) has been questioned in later research — cognition doesn't always proceed strictly bottom-up
through the levels. What has held up well, and is what this platform actually relies on, is the
taxonomy's usefulness as a **descriptive vocabulary** for the *kind* of cognitive task a piece of
content demands — distinguishing "recall a fact" from "apply a concept to a new situation" from
"evaluate which of two approaches is better" is a genuinely useful and widely validated distinction
for instructional design, independent of whether the levels form a strict hierarchy.

## Why Smart App Uses It

Generation Engine could, unconstrained, produce content that's overwhelmingly at the Remember level
(the easiest to auto-generate and auto-grade) even for Learning Nodes that call for Apply- or
Analyze-level understanding. Bloom's Taxonomy gives Generation Engine and its Validation step
(`docs/prompt-engineering/prompt-review-checklist.md`) an explicit vocabulary to check content
variety against, rather than defaulting to whatever's easiest to generate.

## Where It Appears in the Product

Quiz question design (explicitly targeting a stated Bloom's level, not left implicit); Knowledge
Engine's extraction can tag a Learning Node with the cognitive level its source material actually
calls for (a formula-application Learning Node targets Apply; a comparative-reasoning Learning Node
targets Analyze or Evaluate), informing what content types are appropriate for it.

## Engineering Implications

`GenerationTask` (`docs/domain-model/generation-entities.md`) should carry a Bloom's-level field
alongside difficulty and content type — difficulty and cognitive level are related but distinct
(a Remember-level question can still be made harder or easier within that level), and conflating
them loses information Generation Engine needs for good target selection.

## Product Implications

Product Session summaries or progress views (`specs/student-progress.md`) could report cognitive
variety, not just Confidence — a Student who's only ever been asked Remember-level questions on a
Learning Node has different needs than one who's already demonstrated Apply-level understanding,
even at similar Confidence scores.

## Prompt Implications

Content-generation prompts should be given an explicit target Bloom's level and instructed to design
the task around that specific cognitive demand — "explain why" (Understand/Analyze) requires
different prompt construction than "solve this" (Apply), and a prompt that doesn't specify which is
wanted tends to default to the easiest, lowest level.

## Related Documents

`docs/pedagogy/pareto-principle.md`, `docs/domain-model/generation-entities.md`,
`docs/prompt-engineering/quiz-generation-prompt.md`.
