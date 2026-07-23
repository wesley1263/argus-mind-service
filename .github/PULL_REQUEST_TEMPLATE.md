## Summary

<!-- One or two sentences: what changed and why. -->

## Specification Reference

- **Spec:** `specs/NNN-name.md` (or "None — no behavior spec applies, see justification below")
- **Related ADR(s):** `adr/ADR-NNN-...` (or "None")

## Acceptance Criteria Covered

- [ ] AC1 — …
- [ ] AC2 — …

## Architecture Checklist

- [ ] No Engine package imports another Engine's package directly (`CLAUDE.md`).
- [ ] Cross-Engine access goes through a published contract, not another Engine's internals.
- [ ] No internal-only concept (e.g. `Chunk`) crosses its owning Engine's boundary (ADR-002).
- [ ] Any contract change is additive, or is breaking **and** accompanied by an ADR.
- [ ] Every business-concept name matches `memory/glossary.md` exactly; new concepts have a
      glossary entry in this PR.
- [ ] Nothing writes to Learning State/Confidence directly, or mutates an existing Evidence record
      (ADR-003, ADR-004).
- [ ] Any path that could deliver Generation Engine output to a Student passes through Validation
      (ADR-005).

## Security

- [ ] No secrets, credentials, or tokens introduced in code, config, or commit history.
- [ ] Auth checks added for any new endpoint touching Student data.
- [ ] Untrusted input is validated at the boundary before reaching domain logic or storage.

## Tests

- [ ] Tests were written Red-first and confirmed to fail without this change's implementation.
- [ ] `docker compose run --rm test` passes locally.
- [ ] `docker compose run --rm lint` passes locally.
- [ ] Any changed/new Engine contract has a contract test covering both producer and consumer.

## Changelog

- [ ] `CHANGELOG.md` updated (Keep a Changelog format).

## Definition of Done

- [ ] This spec's own Definition of Done section (if applicable) satisfied in full.
- [ ] Reviewed by the `reviewer` subagent, validated by the `tester` subagent.
