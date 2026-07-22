# Review Checklist

> Audience: whoever (human or AI agent) is reviewing a proposed change.
> Use this checklist while reviewing. For the author's own pre-submission self-check, see
> `definition-of-done.md` — that document is the exit criteria the author checks before asking for
> review; this one is what the reviewer independently verifies.

A change should not be approved until every applicable box below is genuinely true. "Looks fine to
me" is not a substitute for checking against the Constitution.

## 1. Traceability

- [ ] The change is traceable to a Spec (and to an ADR, if the decision was irreversible or
      cross-Engine per `development-principles.md` §2).
- [ ] The change does not silently do more than the Spec asked for (Article VIII —
      no speculative generality).
- [ ] If the change introduces a decision that *should* have had an ADR and doesn't, review stops
      here until one is written.

## 2. Boundaries (Article III)

- [ ] No Engine package imports another Engine's package directly.
- [ ] Cross-Engine access happens only through a published contract in `Engine Contracts`.
- [ ] Chunk (or any other Engine's declared internal-only concept) does not appear outside its
      owning Engine.
- [ ] If a contract changed: is it additive (no ADR needed) or breaking (ADR required, see
      `development-principles.md` §4)? A breaking change without an ADR is blocked.

## 3. Ubiquitous Language (Article VI)

- [ ] Every business-concept name in code, tests, comments, logs, and docs matches
      `/glossary/README.md` exactly.
- [ ] Any new business concept has a corresponding glossary entry added in this same change.
- [ ] No internal-only concept leaks into a public contract, API, or Student-facing surface.

## 4. Evidence / Learning State Integrity (Article IV)

- [ ] Nothing writes to Learning State or Confidence directly; both are only ever computed from
      Evidence.
- [ ] Nothing mutates or deletes an existing Evidence record; corrections are new records.
- [ ] Nothing mutates or deletes any other fact-of-record (a completed Session, a delivered
      Generation Task) in place.

## 5. Validation Gate (Article V)

- [ ] Any new path that could deliver Generation Engine output to a Student passes through
      Validation.
- [ ] A Validation failure is handled explicitly (fallback or retry) — it is never silently
      bypassed or swallowed.

## 6. Code Quality (`coding-philosophy.md`)

- [ ] Business logic is expressed as pure functions with I/O pushed to the edges, where the
      Engine's implementation exists to support this.
- [ ] No unexplained cleverness — a future reader with none of today's context can follow it on
      one read.
- [ ] No abstraction was introduced to serve a hypothetical future case rather than the current
      spec.
- [ ] Error handling exists where the code has context to act on a failure, and is absent where a
      condition genuinely cannot occur.

## 7. Tests

- [ ] The functional core has unit tests that read as documentation of a business rule, not as
      coverage padding.
- [ ] Any changed or new contract has a contract test covering both producer and consumer
      expectations.
- [ ] Tests actually fail if the behavior they claim to protect is broken (verified by reading the
      test, not assumed from its name).

## 8. Documentation

- [ ] Any document whose described behavior changed (architecture, glossary, workflow, ADR index)
      is updated in this same change, not deferred.
- [ ] Diagrams affected by the change (see `architecture.md`, `docs/`) still match reality.

## 9. Definition of Done

- [ ] The author's own `definition-of-done.md` checklist is satisfied — spot-check it rather than
      trusting it blindly.

---

If any unchecked box cannot be resolved by a small follow-up in the same change, the change is not
approved yet. Approving "with follow-up TODOs" for boundary, language, or integrity violations
(sections 2–5) is not acceptable — those are Constitutional, not stylistic.
