# Future Evolution

Deliberately deferred capabilities — vision, why they're not part of the current foundation, and
what they'd build on. None of these are specced (`specs/`); each is a candidate for a future
`templates/epic.md` once its prerequisites exist. Per Article VIII of `.ai/constitution.md`, none of
this should be built speculatively ahead of need — this document exists so the idea isn't lost, not
as a backlog to start on.

## Knowledge Graph Evolution

> Distinct from the current, per-Document Knowledge Graph (`docs/domain-model/knowledge-entities.md`)
> — this is about what the graph becomes at larger scale.

**Vision**: a graph that merges structure across Documents, Students, and eventually institutions —
so a Learning Node extracted from one school's textbook can be recognized as the same concept in
another's, compounding the platform's value as more material is ingested anywhere.

**Prerequisite work**: the cross-Document near-duplicate detection already flagged as Future Work in
`specs/knowledge-engine.md` and monitored by `automation/ai-automation/knowledge-base-validation.md`
needs to mature into an actual merging capability first.

## Recommendation Engine

**Vision**: proactive suggestion of *which Document or Chapter* a Student should study next, beyond
the current scope (which only sequences content *within* material already assigned) —
informed by Learning Path (`docs/domain-model/knowledge-entities.md`) generalized across a Student's
entire curriculum rather than one Chapter at a time.

**Prerequisite work**: sufficient Evidence Growth (`docs/evaluation/README.md`) across enough
Students and Documents to make cross-Document recommendations trustworthy rather than speculative.

## Collaborative Learning

**Vision**: Students working through related Learning Nodes together — paired Feynman-technique
explanation exchanges, shared Games — while keeping each Student's own Learning State and Evidence
strictly individual (never averaging or sharing Confidence across Students, which would violate the
per-Student estimation model at the heart of `docs/domain/learning-state-engine.md`).

**Prerequisite work**: a real-time Session model beyond the current bounded, single-Student Session
(`specs/evidence-engine.md`).

## Teacher Dashboard

**Vision**: an aggregate, privacy-respecting view for a teacher of class-wide Confidence and
common struggle points — directly serving the Teacher Feedback metric
(`docs/evaluation/README.md`) by giving teachers legible operational value from the same data that
metric already surveys them about qualitatively.

**Prerequisite work**: aggregation and anonymization rules that never expose one Student's raw
Evidence to another party without consent — a Constitutional-adjacent concern (Article IV) that
would need its own spec and likely an ADR before any dashboard is built.

## Parent Dashboard

**Vision**: similar to Teacher Dashboard, scoped to one Student, for a parent — framed around
"where your child is building strength" per "the child is never blamed"
(`docs/domain-rules/README.md`), never as a report card.

**Prerequisite work**: the same privacy/consent groundwork as Teacher Dashboard, plus explicit
product design work on framing (Parent Satisfaction, `docs/evaluation/README.md`, is sensitive to
exactly this).

## Offline Mode

**Vision**: Study Sessions usable without connectivity, syncing Evidence once reconnected.

**Prerequisite work**: this is in real tension with `docs/domain/study-session.md`'s point that the
Adaptive Loop "genuinely requires a round trip per interaction" — offline mode would need a
principled answer for what content can be pre-fetched safely (already-Validated content for likely
next Learning Nodes) versus what must wait for connectivity, which is a real design problem, not
just an engineering one.

## Local LLM

**Vision**: running Generation Engine's model on-device or on institution-controlled infrastructure,
for cost, latency, or data-sovereignty reasons.

**Prerequisite work**: none pedagogically — this is purely an implementation-technique swap, exactly
the kind Article I of `.ai/constitution.md` is designed to make painless. The prerequisite is
engineering readiness (a local model meeting the same `docs/prompt-engineering/prompt-evaluation-metrics.md`
bar as the current provider), not a new pedagogical capability.

## Voice Tutor

**Vision**: real-time spoken interaction, beyond the transcribe-then-evaluate scope of
`docs/prompt-engineering/audio-evaluation-prompt.md` — a full conversational modality.

**Prerequisite work**: real evaluation data on whether spoken interaction adds retention value beyond
typed interaction (`docs/pedagogy/motor-encoding.md`'s honest uncertainty here) before investing in
real-time infrastructure for a modality whose pedagogical benefit isn't yet established for this
platform specifically.

## Personalized Curriculum

**Vision**: rather than following an institution-assigned syllabus, a fully personalized sequence
across an entire subject, generated from a Student's goals and demonstrated Knowledge Graph coverage.

**Prerequisite work**: ChapterPlan (`docs/domain-model/generation-entities.md`) generalized beyond
one Chapter, plus product/business decisions about how this relates to institutional curricula this
platform currently assumes as given context.

## Long-Term Memory

**Vision**: modeling retention over months or years, beyond the current decay window
(`docs/domain/learning-state-engine.md`), to support use cases like "am I ready for a cumulative
exam" or "what do I still remember from last year."

**Prerequisite work**: enough longitudinal Evidence Growth to calibrate long-horizon decay
parameters against real data rather than extrapolating the current short-horizon model, which
`docs/pedagogy/spaced-repetition.md`'s evidence base doesn't directly support at that timescale.

## Related Documents

`docs/domain-principles.md`, `.ai/constitution.md` Article VIII, `templates/epic.md`,
`templates/rfc.md`.
