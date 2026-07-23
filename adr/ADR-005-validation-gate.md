# ADR-005: All Generation Engine Output Must Pass Through a Validation Gate Before Reaching a Student

- **Status:** Accepted
- **Date:** 2026-07-21
- **Deciders:** Founding engineering (Phase 1 — Foundation)

## Context

Generation Engine produces content — questions, explanations, exercises, feedback — using
generative techniques whose output is not guaranteed to be correct, safe, or pedagogically sound for
a given Student and Learning Node. Because `PROJECT.md` establishes that the AI
technique is not the product and must remain swappable, content quality cannot be treated as
something a specific generation technique happens to guarantee — it has to be an architectural
property of the system, independent of whichever technique is in use at a given time.

## Decision

Every Generation Task's output must pass through a mandatory **Validation** stage before it is
eligible for delivery to a Student. Validation is a distinct, addressable step — not an inline
check folded into generation — with its own explicit pass/fail outcome and reason. Output that
fails Validation must never be delivered; the Generation Engine must produce a fallback or retry
instead. This gate sits structurally between Generation Engine and any Session, as shown in
`CLAUDE.md (Architecture section)` §3. See also the Validation rule in `CLAUDE.md`.

## Consequences

- **A hard safety/quality gate independent of technique:** whatever generates the content — today's
  approach or a future replacement — the same gate applies, so content quality doesn't silently
  regress when the generation technique changes.
- **Validation can evolve independently:** new checks can be added, existing ones tightened, without
  touching how content is generated, because the two are separate, explicitly connected steps.
- **Testable and auditable:** because Validation is an explicit step with its own outcome, "why was
  this content blocked" is always answerable, and Validation behavior itself can be tested in
  isolation from generation.
- **New cost — latency:** Validation adds a step to the path between deciding to generate something
  and delivering it. The Generation Engine will need to plan for this cost (e.g., pre-generation,
  caching passed content ahead of when it's needed) as a design concern of its own.
- **New obligation — Validation needs real specification per content type.** This ADR establishes
  that the gate exists and is mandatory; it does not specify what Validation checks for a given
  content type, which is deliberately left to future, more specific specs as content types are
  built out.

## Alternatives Considered

- **Trust-by-default generation with post-hoc spot audits** (generate and deliver immediately,
  sample and review after the fact). Rejected: this accepts an unacceptable risk of unsound or
  unsafe content reaching a Student before anyone catches it, which directly contradicts the Validation rule
  of the Constitution and undermines trust in the entire adaptive loop — a Student who receives one
  clearly wrong piece of content has reason to distrust every subsequent one.
