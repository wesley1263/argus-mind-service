# Skill: Domain-Driven Design

How DDD concepts map onto this specific codebase. This is not a general DDD tutorial — it's a
translation table from DDD vocabulary to the concrete things already defined in `.ai/` and
`glossary/`, so those documents can be applied with DDD's full toolkit rather than re-derived.

## Bounded Contexts Are the Five Engines, Plus Platform

Ingestion, Knowledge, Evidence, Learning State, and Generation are each a **bounded context** in
the DDD sense: each has its own model of the world, its own internal language where it needs one
(Chunk exists only inside Ingestion Engine's context), and an explicit translation at its edges
(the published contract) when talking to another context. `platform/` (Authentication, cross-cutting
infrastructure) is a supporting subdomain, not a core bounded context — see
`specs/authentication.md`.

This is exactly what `.ai/architecture.md` calls "Engines," described in DDD's vocabulary. The two
documents agree by construction — `.ai/architecture.md` is the operational description,
`glossary/README.md` is the ubiquitous language DDD says every bounded context needs.

## Ubiquitous Language Is Not Optional Here

`glossary/README.md` **is** this project's ubiquitous language, formally. Article VI of
`.ai/constitution.md` is DDD's ubiquitous-language discipline stated as a binding project rule, not
a suggestion: the same word means the same thing in a conversation with a stakeholder, in a spec, in
a test name, and in a variable name.

## Entities, Value Objects, and Events in This Domain

- **Entities** (identity persists across change): `Student`, `Learning Node`, `Session`, `Document`.
  Each has an identifier and a lifecycle (see the State Diagram in each spec).
- **Value Objects** (defined entirely by their attributes, immutable, no identity of their own):
  `Confidence` (a score, always recomputed, never itself having identity or history of its own edits
  — see ADR-004), `Knowledge Edge` (defined by its endpoints and type). A value object should be
  trivially comparable by value in tests — if you find yourself needing an id to compare two
  `Confidence` instances, it has drifted into being modeled as an entity by mistake.
- **Domain Events**: `Evidence` itself is best understood as a domain event, not a mutable entity —
  it records that something happened, once, immutably (ADR-003). The `Events` sections across
  `/specs/` (`EvidenceRecorded`, `LearningStateUpdated`, `GenerationTaskCompleted`, …) are the
  formalized, cross-context version of the same idea: something happened, stated in past tense, that
  another bounded context may react to.

## Aggregates

An aggregate is the consistency boundary DDD draws around a cluster of entities/value objects that
must change together. In this codebase:

- **Knowledge Graph** is not one giant aggregate — a `Learning Node` together with its *own*
  outgoing `Knowledge Edges` is the practical consistency boundary (adding an edge must keep that
  node's local structure acyclic, per `specs/knowledge-engine.md`); the graph as a whole is queried,
  not transactionally modified as one unit.
- A **Session** is the aggregate root for the Evidence recorded within it during capture (they're
  created together), even though Evidence rows persist independently and immutably afterward
  (ADR-003) — the aggregate boundary governs the *write*, not eternal ownership.

If you're modeling a new entity and unsure where its aggregate boundary is, ask: **what must be
transactionally consistent together, right now, at write time?** That boundary — not "everything
that's conceptually related" — is the aggregate.

## Anti-Corruption Layer

Generation Engine's `adapters/` calling an external generative AI vendor is a textbook
anti-corruction layer: the vendor's request/response shape never leaks past that adapter (see
`skills/prompt-engineering.md`) — `application/` and `domain/` only ever see this project's own
`Generation Task` / content types. The same pattern applies to any other external system this
platform integrates with in the future.

## Where This Differs From "Textbook" DDD

This project does not use CQRS, event sourcing as a storage mechanism, or a shared domain-event bus
between Engines by default — Evidence's immutability (ADR-003) gives us some of event sourcing's
benefits (full history, replayable projections) without adopting it as a general pattern everywhere.
Don't import heavier DDD machinery than a spec actually calls for — see Article VIII of
`.ai/constitution.md`.
