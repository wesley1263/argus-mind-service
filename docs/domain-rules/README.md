# Domain Rules

Every rule below is stated in plain language, then justified from both a business and a pedagogical
angle, then translated into a concrete engineering consequence. These rules are the operational
form of `docs/domain-principles.md` — where that document states the *why* permanently, this
document states the *what, specifically* in a form a spec or a reviewer can check against.

A rule here that's violated is not a bug to triage casually — it's a sign either the implementation
or the rule itself needs to change, and the latter requires updating this document deliberately
(`.ai/development-principles.md` §9), never just working around it silently.

---

## A Validated Learning Node Never Becomes Invalid

**Business motivation**: reversing a validated Learning Node's status would make the Knowledge Graph
untrustworthy as shared ground truth — anything built on top of it (a Student's Learning Path, past
Generation Tasks) would need to be re-examined.

**Pedagogical motivation**: a concept that was correctly identified and structurally sound doesn't
stop being a real concept. If extraction quality improves later, the correct response is to
supersede or deprecate the node with a better one, not to declare the original one to have been
wrong all along.

**Engineering consequence**: `specs/knowledge-engine.md`'s state diagram has no transition from
`Published` back to an invalid state — only forward to `Deprecated`, which is a distinct status from
"invalid." Downstream references (Evidence, Learning State) to a Deprecated node remain valid
historical records.

---

## Learning State Is Estimated

**Business motivation**: presenting Learning State as certain would create false confidence in
product decisions built on it (e.g., a Teacher Dashboard, `docs/future/README.md`) and erode trust
the moment it's visibly wrong.

**Pedagogical motivation**: no assessment method — formative or summative — measures understanding
with certainty; treating an estimate as fact contradicts basic psychometric humility.

**Engineering consequence**: Learning State's API (`specs/learning-state-engine.md`) always returns
a probabilistic Confidence value with its model version, never a boolean "knows it / doesn't know
it," and UI/prompt consumers must be designed to handle a continuous, uncertain value.

---

## Confidence Is Never Binary

**Business motivation**: a binary mastery flag would force premature, hard-to-reverse product
decisions (e.g., permanently "locking" a Learning Node as done).

**Pedagogical motivation**: understanding is continuous and decays (`docs/pedagogy/spaced-repetition.md`)
— a snapshot binary judgment is stale the moment it's made.

**Engineering consequence**: `Confidence` is stored and transmitted as a float in `[0.0, 1.0]`
(`docs/domain-model/learning-state-entities.md`); no schema, contract, or UI treats it as a boolean.

---

## False Negatives Are Worse Than False Positives

**Business motivation**: a false negative wastes a Student's limited time and attention revisiting
material they've already learned, directly undermining the efficiency claim at the heart of the
product (`docs/domain/product-philosophy.md`).

**Pedagogical motivation**: a false positive self-corrects quickly (the next attempt at that node
surfaces the real gap); a false negative doesn't self-correct — nothing prompts the system to
re-examine a Learning Node it already believes is understood.

**Engineering consequence**: the confidence model (`docs/domain/learning-state-engine.md`) should be
tuned/evaluated with this asymmetry explicit — `testing/golden-dataset.md` scenarios should include
cases that specifically penalize false negatives more heavily than false positives, not weight both
error types equally.

---

## The Child Is Never Blamed

**Business motivation**: a Student (or parent) who feels judged disengages — directly harming
Voluntary Return Rate (`docs/evaluation/README.md`).

**Pedagogical motivation**: attribution research on feedback framing consistently finds that framing
struggle as a fixed trait of the learner ("you're bad at this") is worse for motivation and future
effort than framing it as information about the material or the moment
(`docs/pedagogy/feynman-technique.md`'s Feedback localization approach).

**Engineering consequence**: Feedback content (`docs/domain-model/generation-entities.md`) and all
Validation criteria for it explicitly check tone — Feedback is generated and Validated with
instructions to describe the *gap*, never the *Student*, per
`docs/prompt-engineering/prompt-review-checklist.md`.

---

## Only Problematic Learning Nodes Are Regenerated

**Business motivation**: wholesale regeneration wastes computation and cost
(`automation/ai-automation/llm-cost-monitoring.md`) on content that already worked.

**Pedagogical motivation**: "Small Improvements Over Perfect Lessons" (`docs/domain-principles.md`
§7) — targeted fixes preserve the Evidence and Confidence already earned on unrelated Learning
Nodes.

**Engineering consequence**: Reinforcement (`docs/domain-model/generation-entities.md`) is scoped to
a specific low-Confidence Learning Node, never triggers regeneration of an entire SessionPlan or
ChapterPlan.

---

## Knowledge Always Evolves Incrementally

**Business motivation**: a Knowledge Graph that requires wholesale re-authoring to improve doesn't
scale — the platform's value compounds only if adding more Documents makes it better without
disruptive rework.

**Pedagogical motivation**: real expertise is built incrementally (`docs/pedagogy/chunking.md`'s
chess-expertise research) — the platform's own knowledge representation should grow the same way.

**Engineering consequence**: Knowledge Engine's extraction (`specs/knowledge-engine.md`) is additive
and idempotent per Document — ingesting a new Document extends the graph without requiring
re-extraction of existing, unrelated Learning Nodes.

---

## Paper Is Immutable

**Business motivation**: allowing in-place edits to ingested source material would make it
impossible to know which version of a Document any given Learning Node or piece of Evidence was
actually derived from.

**Pedagogical motivation**: the source material a Student was taught from is a fact of history —
"the paper" (the Document) doesn't retroactively change just because a newer edition exists.

**Engineering consequence**: `specs/document-ingestion.md`'s explicit Non-goal ("editing Documents
in-platform") — a changed source is ingested as a **new** Document, never an edit to an existing
one, mirroring Evidence's own immutability discipline (ADR-003) at the Ingestion Engine layer.

---

## Evidence Accumulates Forever

**Business motivation**: deleting or compacting Evidence destroys the ability to explain any
Confidence value, undermining trust with Students, parents, and teachers
(`docs/evaluation/README.md`).

**Pedagogical motivation**: a Student's full learning history — including struggle, not just
eventual success — is pedagogically meaningful; discarding it discards the ability to see growth
over time.

**Engineering consequence**: ADR-003's append-only discipline, enforced at the database grant level
(`skills/postgres.md`, `testing/architecture-testing.md`). Storage-growth management (retention,
archival) is addressed as its own future concern, explicitly never by deleting or summarizing away
original records.

---

## Related Documents

`docs/domain-principles.md`, `.ai/constitution.md`, ADR-002, ADR-003, ADR-004, ADR-005,
`docs/domain-model/README.md`.
