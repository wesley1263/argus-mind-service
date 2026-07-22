## Summary

<!-- One or two sentences: what changed and why, in terms a reviewer with no prior context can follow. -->

## Specification Reference

- **Spec:** `specs/<name>.md` (or "None — this PR does not implement a spec, see justification below")
- **Related ADR(s):** `adr/ADR-XXX-...` (or "None — no irreversible/cross-Engine decision involved")

<!-- A PR implementing behavior not traceable to a spec is a review blocker per .ai/constitution.md
Article II, unless it's a pure refactor (prompts/refactor.md) or a fix with no behavior spec of its
own — state which case applies if there's no spec. -->

## Acceptance Criteria Covered

<!-- Copy the relevant AC identifiers from the spec and check off what this PR satisfies. If this PR
only partially implements the spec, say so explicitly rather than leaving ACs unchecked with no
explanation. -->

- [ ] AC1 — …
- [ ] AC2 — …

## Architecture Checklist

- [ ] No Engine package imports another Engine's package directly (`.ai/architecture.md` §4).
- [ ] Any cross-Engine access goes through a published contract (Shared Kernel / Engine Contracts).
- [ ] No internal-only concept (e.g., Chunk) crosses its owning Engine's boundary (ADR-002).
- [ ] Any contract change is additive, or is breaking **and** accompanied by an ADR
      (`.ai/development-principles.md` §4).
- [ ] Every business-concept name matches `/glossary/README.md` exactly; new concepts have a
      glossary entry in this PR.
- [ ] Nothing writes to Learning State/Confidence directly, or mutates an existing Evidence record
      (ADR-003, ADR-004).
- [ ] Any path that could deliver Generation Engine output to a Student passes through Validation
      (ADR-005).

## Security

- [ ] No secrets, credentials, or tokens introduced in code, config, or commit history.
- [ ] Auth/authorization checks added for any new endpoint touching Student data
      (`specs/authentication.md`).
- [ ] Untrusted input (uploads, free-text Evidence, generative output) is validated at the boundary
      before reaching domain logic or storage.
- [ ] `prompts/security-review.md` was run for anything touching auth, untrusted input, or the
      Validation gate — link the review if applicable.

## Performance

- [ ] Any Non-Functional Requirement in the spec (latency/throughput) was checked, not just
      functional correctness.
- [ ] No N+1 query pattern or per-item cross-Engine contract call introduced in a loop
      (`skills/postgres.md`, `skills/architecture.md`).
- [ ] `prompts/performance-review.md` was run if this PR touches a latency-sensitive path
      (Generation Engine, Study Session) — link the review if applicable.

## Tests

- [ ] Tests were written Red-first (`CLAUDE.md`'s mandatory workflow) and are confirmed to fail
      without this change's implementation.
- [ ] `docker compose run --rm test` passes locally.
- [ ] `docker compose run --rm lint` passes locally.
- [ ] Any changed/new Engine contract has a contract test covering both producer and consumer
      (`testing/integration-testing.md`).

## Changelog

- [ ] `CHANGELOG.md` updated (Keep a Changelog format, `templates/release.md`).

## Definition of Done

- [ ] `.ai/definition-of-done.md` satisfied in full.
- [ ] This spec's own Definition of Done section (if applicable) satisfied in full.
