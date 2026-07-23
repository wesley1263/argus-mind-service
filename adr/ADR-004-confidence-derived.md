# ADR-004: Confidence Is a Derived, Recomputable Projection — Never a Source of Truth

- **Status:** Accepted
- **Date:** 2026-07-21
- **Deciders:** Founding engineering (Phase 1 — Foundation)

## Context

ADR-003 established that Evidence is the only thing written directly and trusted as fact. This ADR
makes the corollary explicit and binding on its own, because it is the rule most likely to be
quietly violated under real-world pressure: it will always be *tempting* to add a direct write path
for Confidence — a support tool to "fix" a Student's score that looks wrong, an admin override for
a disputed case, a quick manual correction during an incident. Each of those, taken individually,
looks like a small, reasonable exception.

## Decision

Confidence values are projections computed by the Learning State Engine as a pure function of
`(Student, Learning Node, Evidence up to time T, confidence model version)`. **No component may set
a Confidence value directly** — not application code, not an internal admin tool, not a migration,
not a support workflow. Every correction to what the system believes about a Student happens by
recording new, explicit Evidence (per ADR-003), which then flows through the normal computation,
never by writing to Confidence itself.

## Consequences

- **One computation path, always.** There is never a question of whether a given Confidence value
  came from "the real algorithm" or "a manual fix" — it is always the former, which keeps the
  system's behavior explainable and consistent.
- **Safe caching:** because Confidence is always a projection, it can be cached and invalidated like
  any derived view, without ever becoming a second source of truth that could drift from the
  Evidence it's supposed to represent.
- **New obligation — the confidence model must be explicit and versioned.** Since there's no manual
  escape hatch, the Learning State Engine's computation itself has to be good enough, and
  changeable enough (via new model versions, not via bypassing the model) to handle the edge cases
  a manual override would otherwise have patched over.
- **New obligation — a recompute/backfill capability is required** for whenever the confidence
  model changes, so that historical Learning State can be re-derived under the new model rather
  than left inconsistent.

## Alternatives Considered

- **Allow a manual override of Confidence for edge cases**, gated behind admin permissions, for
  situations the algorithm handles badly. Rejected: this directly breaks the "Evidence is truth"
  principle (the Evidence-is-truth rule in `CLAUDE.md`) — the moment one write path exists outside the
  computation, every Confidence value's trustworthiness becomes conditional ("...unless someone
  overrode it"), which is a much larger cost than the inconvenience of handling edge cases through
  Evidence instead. The correct way to handle a genuine edge case is to record the corrective fact
  as Evidence and let it flow through the same path as everything else.
