# Active Recall

## What It Is

Active recall is the cognitive act of retrieving information from memory through deliberate effort
— answering a question, stating a definition unprompted — as opposed to passively recognizing it
(e.g., picking a correct answer from options that jog recognition, or re-reading a passage). The
distinguishing feature is that the learner's brain does the work of reconstruction, rather than the
material doing the work of re-presentation.

> **Relationship to Retrieval Practice**: Active Recall names the cognitive *act*; Retrieval
> Practice (`docs/pedagogy/retrieval-practice.md`) names the deliberate *pedagogical strategy* of
> structuring study around repeated recall attempts. Every instance of Retrieval Practice involves
> Active Recall; not every instance of Active Recall (e.g., a single spontaneous self-quiz) is part
> of a designed Retrieval Practice strategy. This document covers the cognitive mechanism; that one
> covers how Smart App schedules and structures it.

## Scientific Evidence

The **testing effect** (Roediger & Karpicke, 2006, and a large replicated literature since) shows
that retrieving information from memory — even without feedback — produces better long-term
retention than an equivalent amount of time spent re-studying the same material. This holds even
though learners typically *predict* re-reading will be more effective than testing themselves — a
robust metacognitive miscalibration (see `docs/pedagogy/metacognition.md`) that Smart App's design
has to work around rather than defer to.

## Why Smart App Uses It

Passive content consumption (reading a Summary, watching an explanation) is the easiest failure mode
for a learning product to fall into, because it's what Students often prefer in the moment. Active
recall is the platform's primary counterweight — nearly every content type that isn't explicitly
introductory (Priming-stage) is designed to require the Student to produce something, not just
receive it.

## Where It Appears in the Product

Quiz content directly (`docs/domain-model/generation-entities.md`); Feynman-technique prompts
(`docs/pedagogy/feynman-technique.md`) requiring an unprompted explanation; MindMap completion tasks
where the Student fills in relationships rather than viewing a finished map.

## Engineering Implications

Evidence Engine must capture *how* an answer was produced, not just whether it was correct
(`EvidenceType`, `docs/domain-model/evidence-entities.md`) — a Student who recognized a correct
option among distractors generated weaker Evidence of understanding than one who typed a free-form
answer, and Learning State's confidence model should be capable of weighting these differently.

## Product Implications

Content types should default toward recall-requiring formats over recognition-requiring formats
(e.g., open-response over multiple-choice) wherever the content type reasonably allows it, even
though recall-requiring formats are harder to auto-grade — this is a case where product
convenience and pedagogical soundness are in tension, and the principle should win by default.

## Prompt Implications

Generation prompts (`docs/prompt-engineering/quiz-generation-prompt.md`) should be explicitly
instructed to prefer generating a question the Student must answer from memory over one that merely
asks them to select a correct restatement, unless the content type specifically calls for
recognition (e.g., early Priming-stage content, where recall would be premature).

## Related Documents

`docs/pedagogy/retrieval-practice.md`, `docs/pedagogy/spaced-repetition.md`,
`docs/pedagogy/metacognition.md`, `docs/domain/evidence-engine.md`.
