# ADR-003: Evidence Is an Immutable, Append-Only Log

- **Status:** Accepted
- **Date:** 2026-07-21
- **Deciders:** Founding engineering (Phase 1 — Foundation)

## Context

Learning State — and specifically Confidence — is the platform's belief about what a Student knows,
and that belief drives what content they see next. Students and instructors will, reasonably,
sometimes question a Confidence value ("why does it think I don't understand this?"). Separately,
the algorithm or model used to compute Learning State from Evidence is expected to improve over
time, and we will want to know how a Student's trajectory would look under a new computation
without losing the ability to explain how it looked under the old one.

Both needs — explainability and safe iteration on the computation itself — depend on being able to
reconstruct, at any point, exactly what was known and when.

## Decision

Evidence records are **immutable and append-only** once written by the Evidence Engine. Learning
State is never written to directly by anyone — it is always derived by aggregating Evidence for a
Student, as of some point in time. A correction to the record (a mis-recorded answer, a retracted
self-assessment) is modeled as a **new** Evidence record — for example, one that explicitly
invalidates or supersedes an earlier one — never as an edit or deletion of the original. See
`memory/glossary.md` (Evidence) and the Evidence-is-truth rule in `CLAUDE.md`.

## Consequences

- **Full auditability:** any Confidence value can always be explained by pointing at the exact
  Evidence it was computed from.
- **Safe to change the computation model:** because the underlying Evidence log never changes, the
  Learning State Engine can recompute history under a new confidence model and compare it against
  the old one, enabling safe iteration and even "what would this look like under model v2" analysis.
- **New obligation — storage grows monotonically.** Evidence is never deleted, so retention,
  archival, and compaction strategy will need to be addressed as real volume appears. That is
  explicitly out of scope for this ADR and tracked as a future decision once it's a concrete
  problem rather than a speculative one.
- **New obligation — reads need a strategy for recomputation cost.** Naively replaying all Evidence
  for a Student on every read does not scale forever; the Learning State Engine will need a
  caching/projection strategy (itself just a derived, disposable view over the immutable log, per
  `memory/coding-standards.md` §4) once that becomes a real constraint.

## Alternatives Considered

- **A mutable Student profile with directly-updated mastery fields**, updated in place as new
  interactions occur. Rejected: it destroys auditability (no way to say why a value is what it is),
  and it makes changing the computation model dangerous, since there would be no way to distinguish
  "old data under the old model" from "new data under the new model" — everything would just be
  today's mutated value with no history behind it.
