# Definition of Done

> Audience: whoever (human or AI agent) is about to declare a piece of work finished and ask for
> review. This is the objective, checkable exit criteria for a unit of work — not a description of
> the review process itself (see `review-checklist.md` for what the reviewer independently
> verifies).

A task is **Done** only when every applicable statement below is true. If something is not
applicable (e.g., no contract changed), say so explicitly rather than silently skipping it.

## Specification

- [ ] A Spec exists and describes the delivered behavior in ubiquitous-language terms.
- [ ] If the work involved an irreversible or cross-Engine decision, an ADR exists and is linked
      from the change (`development-principles.md` §2).
- [ ] The delivered behavior matches the Spec exactly — nothing more, nothing less
      (Article VIII of the Constitution).

## Correctness

- [ ] The behavior described in the Spec is implemented and demonstrably works (tests, or for
      foundation/documentation work, a coherence check against related documents).
- [ ] Edge cases explicitly called out in the Spec are handled; edge cases *not* called out are not
      speculatively handled (no scope creep).

## Boundaries and Language

- [ ] No Engine boundary was crossed except through a published contract.
- [ ] Every new business concept has a glossary entry in `/glossary/README.md`.
- [ ] No internal-only concept (e.g., Chunk) is exposed outside its owning Engine.

## Data Integrity

- [ ] No change writes to Learning State or Confidence directly, or mutates an existing Evidence
      record or other fact-of-record in place.

## Validation

- [ ] Any new or changed path that can deliver Generation Engine output to a Student routes through
      Validation, with an explicit handling for the failure case.

## Tests

- [ ] Tests exist for the new/changed behavior and pass.
- [ ] Existing tests still pass (no unexplained regressions, no tests silently deleted or skipped
      to make the suite pass).
- [ ] Any changed contract has a passing contract test on both producer and consumer sides.

## Documentation

- [ ] Every document describing the changed behavior (architecture, glossary, ADR index, relevant
      `docs/` page) is updated in this same change.
- [ ] Diagrams affected by the change still render correctly and match the described system.

## Hygiene

- [ ] The change is scoped to one logical unit of work — unrelated formatting, renames, or
      refactors are not bundled in.
- [ ] Commit message(s) explain why the change was made, not just what changed.
- [ ] Nothing is left half-implemented: no dead code paths, no TODOs standing in for required
      behavior described in the Spec.

---

**A task that fails any applicable box above is not Done — it is in progress.** Marking it Done
anyway and deferring the gap to "a follow-up" is only acceptable when the gap was explicitly
scoped out of the Spec itself, in writing, before implementation started.
