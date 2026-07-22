# Evaluation

## The Rule This Whole Document Follows

**Never evaluate the LLM. Evaluate learning outcomes.** Model-quality metrics (latency, cost, raw
benchmark scores) matter operationally (`automation/ai-automation/llm-cost-monitoring.md`,
`testing/prompt-evaluation.md`), but they are never treated as evidence the *product* is working.
The product is working when Students learn more efficiently, per the Core Product Thesis
(`docs/domain-principles.md`) — that's a claim about people, not about a model, and it's measured
accordingly.

## Metrics

### Session Completion
The proportion of started Study Sessions that reach a natural end rather than being abandoned
mid-way. Low completion suggests a pacing or difficulty-calibration problem
(`docs/domain/study-session.md`), not necessarily a content-quality problem — these have different
fixes and should not be conflated.

### Voluntary Return Rate
The proportion of Students who start a new Study Session without an external prompt (a reminder,
a parent's instruction). This is the platform's most direct measure of "fun creates habit"
(`docs/domain-principles.md` §10) actually working, as distinct from engagement driven by external
pressure.

### Retention
Long-term Confidence stability for a Learning Node measured well after it was last actively
practiced — the direct test of whether Spaced Repetition (`docs/pedagogy/spaced-repetition.md`) is
producing durable learning rather than short-term performance that fades immediately.

### Confidence Growth
The rate at which Confidence increases toward mastery across a Student's active Learning Nodes,
normalized for how much Evidence has been gathered — a Student improving quickly per unit of effort
is the platform's efficiency claim, made measurable.

### Learning Node Stability
How often a Learning Node's Confidence for a given Student oscillates rather than trending smoothly
— high volatility can indicate a confidence-model miscalibration or genuinely inconsistent
understanding, and distinguishing the two requires looking at this alongside Evidence Growth
(below), not in isolation.

### False Negative Rate
Estimated via targeted golden-dataset-style probes and via Reinforcement outcomes (a Learning Node
that gets Reinforced and immediately shows high performance suggests the low Confidence that
triggered Reinforcement was likely a false negative) — the direct operational measure of
`docs/domain-rules/README.md`'s asymmetric error rule.

### Average Regeneration
The average number of Reinforcement cycles needed before a Learning Node reaches stable, adequate
Confidence. A rising trend across the Student base suggests systematic content-quality problems;
consistently low regeneration for one Student's specific Learning Node suggests that Learning Node's
extraction or content may need review, not the Student.

### Evidence Growth
The rate of Evidence accumulation per Student per Learning Node over time — a leading indicator for
several other metrics (thin Evidence makes Confidence estimates less reliable, which can distort
Learning Node Stability and False Negative Rate readings) and should be checked before trusting a
surprising result in those metrics.

### Student Engagement
A composite view combining Session Completion, Voluntary Return Rate, and time-on-task
(`docs/domain-model/evidence-entities.md`'s `time_on_task` EvidenceType) — reported as its component
parts, not collapsed into a single opaque score, consistent with
`docs/prompt-engineering/prompt-evaluation-metrics.md`'s stance against composite scores that hide
what's actually moving.

### Parent Satisfaction
Direct survey/feedback from parents, interpreted alongside — never instead of — the behavioral
metrics above. Parent satisfaction that diverges sharply from Confidence Growth and Retention data
is itself a useful signal (perhaps of a Feedback tone or communication problem, not a learning
problem) and should be investigated as a discrepancy, not averaged away.

### Teacher Feedback
Qualitative and structured feedback from teachers on whether Students arrive in class visibly
primed (`docs/domain/student-journey.md`'s core validation moment) — this is the metric closest to
directly testing the Core Product Thesis itself, since it's an independent observer's assessment of
the exact outcome the thesis predicts.

## What This List Deliberately Excludes

Model accuracy benchmarks, token-level generation quality scores, and raw AI-capability metrics —
these belong in `testing/prompt-evaluation.md` and `automation/ai-automation/`, as *engineering*
signals for whether Generation Engine's implementation is healthy, not as *product* success
measures. A model that scores well on a generic benchmark and a product that measurably improves
learning outcomes are different claims, and only the second is what this document tracks.

## How These Metrics Relate to Each Other

No single metric here is trusted in isolation — Confidence Growth without checking Evidence Growth
could reflect thin data; Session Completion without Voluntary Return Rate could reflect Sessions
that are easy to finish but that Students don't want to repeat. `automation/workflows/repository-health.md`
and any future Repository/Product Dashboard should always present these metrics as a set, with their
known interdependencies noted, not as independent headline numbers.

## Related Documents

`docs/domain-principles.md`, `docs/domain/product-philosophy.md`, `docs/domain-rules/README.md`,
`testing/prompt-evaluation.md`, `automation/ai-automation/llm-cost-monitoring.md`.
