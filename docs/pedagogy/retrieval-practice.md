# Retrieval Practice

## What It Is

Retrieval Practice is the deliberate pedagogical **strategy** of structuring study around repeated,
scheduled retrieval attempts as the primary vehicle for learning — as distinct from Active Recall
(`docs/pedagogy/active-recall.md`), which names the underlying cognitive act. Retrieval Practice is
what a system does when it decides *how much* of a Student's engagement should consist of retrieval
attempts, and how those attempts are scheduled and sequenced, rather than how any single retrieval
attempt works cognitively.

## Scientific Evidence

Roediger & Butler's (2011) synthesis frames retrieval practice as a *learning* strategy, not merely
an assessment tool — repeated retrieval produces durable, transferable learning gains beyond what a
single test-like event would suggest, and these gains compound when retrieval is spaced
(`docs/pedagogy/spaced-repetition.md`) and interleaved across related material rather than blocked
by topic. This body of work is what shifted testing, in learning-science terms, from "a way to
measure learning" to "a way to produce it."

## Why Smart App Uses It

Because retrieval practice names a *scheduling and structuring* strategy, it's the principle most
directly responsible for Generation Engine's overall content-selection policy
(`specs/generation-engine.md` R1) — not just individual Quiz questions, but the decision to make
retrieval, rather than review, the dominant mode of a Study Session.

## Where It Appears in the Product

SessionPlan composition (`docs/domain-model/generation-entities.md`): the ratio of
retrieval-requiring content types (Quiz, self-explanation) to purely introductory content (Summary,
MindMap for a brand-new Learning Node) within a Session is a retrieval-practice design decision, not
an incidental content mix.

## Engineering Implications

Generation Engine should track, per Student, the historical ratio of retrieval-type to
introduction-type content delivered, and this ratio itself is a signal `docs/evaluation/README.md`
should be able to report on — a Session dominated by introductory content for a Student who's well
past the priming stage is a policy bug, not just a content-quality issue.

## Product Implications

Interleaving — mixing retrieval practice across multiple Learning Nodes rather than blocking all
practice on one node before moving to the next — should be the default SessionPlan shape once a
Student has more than a couple of active Learning Nodes, consistent with the interleaving evidence
cited above.

## Prompt Implications

`docs/prompt-engineering/quiz-generation-prompt.md` and SessionPlan generation should be instructed
to favor variety across Learning Nodes within a Session over exhaustive coverage of one Learning Node
at a time, when both are viable options for the current Learning State.

## Related Documents

`docs/pedagogy/active-recall.md`, `docs/pedagogy/spaced-repetition.md`,
`docs/domain/study-session.md`, `specs/generation-engine.md`.
