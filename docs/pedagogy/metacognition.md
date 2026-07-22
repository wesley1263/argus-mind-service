# Metacognition

## What It Is

Metacognition is thinking about one's own thinking — specifically, a learner's awareness and
accurate self-assessment of what they do and don't understand, and their ability to regulate their
own study strategy in response (Flavell, who coined the term in 1979). **Self-regulated learning**
(Zimmerman's work through the 1980s–2000s) extends this into a practical cycle: planning, monitoring,
and adjusting one's own learning process based on that self-awareness.

## Scientific Evidence

A robust and somewhat uncomfortable finding threads through this literature: learners are often
poorly calibrated about their own understanding, and the least skilled learners tend to be the *most*
overconfident (the broader phenomenon popularly associated with the Dunning-Kruger effect, though
the specific mechanism and magnitude of that effect have been actively debated and partly attributed
to statistical artifacts in later methodological critiques — this document treats it as one
illustrative data point within a larger, more robust body of self-assessment-accuracy research,
not as a settled, precise law). The practical implication that *does* hold up robustly: learners'
self-reported confidence should not be trusted as a proxy for actual understanding, which is exactly
why Learning State is built from Evidence of performance, never from Student self-report alone
(`docs/domain-principles.md` §4, §12).

## Why Smart App Uses It

Metacognition is both something the platform *measures around* (it doesn't trust self-report) and
something it actively *tries to build* in the Student — the Feynman Technique
(`docs/pedagogy/feynman-technique.md`) and elaborative "why" prompts
(`docs/pedagogy/elaboration.md`) are specifically chosen because they improve a Student's own
awareness of their understanding, not just their raw retention.

## Where It Appears in the Product

Any point where a Student is asked to predict their own performance before attempting a task
("how confident are you this is right?") creates an explicit, comparable calibration data point when
matched against actual Evidence — a rich signal for both the Student's metacognitive development and
the platform's Learning State estimate.

## Engineering Implications

If self-reported confidence is captured, it should be recorded as its own `EvidenceType`
(`docs/domain-model/evidence-entities.md`), explicitly distinct from and never substituted for
performance-based Evidence in Learning State's confidence computation
(`docs/domain/learning-state-engine.md`) — self-report is useful as a *comparison point*
against actual Confidence, not as an input to it.

## Product Implications

Where a Student's self-assessment and the system's Confidence estimate diverge significantly, that
divergence itself is worth surfacing gently — not as a correction ("you're wrong about yourself")
but as information ("this might be worth another look"), consistent with "the child is never
blamed" (`docs/domain-rules/README.md`).

## Prompt Implications

Feedback generation should never simply defer to a Student's stated confidence over observed
performance — Feedback is grounded in Evidence and Learning State, with self-reported confidence (if
captured) treated as one more data point to reflect back, not as ground truth.

## Related Documents

`docs/pedagogy/feynman-technique.md`, `docs/pedagogy/active-recall.md`,
`docs/domain/learning-state-engine.md`, `docs/domain-rules/README.md`.
