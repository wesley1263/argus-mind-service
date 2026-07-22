# Unit Testing

## Scope

A unit test exercises `domain/` or `application/` code in complete isolation: no database, no
network, no filesystem, no other Engine, no mocking framework. This is possible — and required —
because that code is written as pure functions per `.ai/coding-philosophy.md` §3. If a "unit test"
needs a mock to run, the code under test has I/O leaking into its core, and the fix is to the code,
not to the test (`skills/testing.md`).

## What Belongs Here

- Domain rules: "a set of Knowledge Edges must not contain a cycle" (`specs/knowledge-engine.md`),
  "Confidence is computed deterministically from Evidence" (`specs/learning-state-engine.md` AC2),
  "target Learning Node selection prefers low-confidence, unlocked nodes"
  (`specs/generation-engine.md` R1).
- Application use case orchestration, tested against fake/in-memory implementations of its ports
  (not a mocking framework — a real, minimal, in-memory class implementing the same interface, so
  the test exercises real object collaboration rather than configured expectations).
- Value object behavior: equality, validation rules on construction (a `Confidence` outside `[0,1]`
  is rejected at construction, not caught downstream).

## What Does Not Belong Here

- Anything that touches a real database, HTTP client, or external model — that's
  `integration-testing.md`.
- Anything asserting on FastAPI request/response wiring — that's an integration concern
  (`skills/fastapi.md`).
- Anything whose assertion is about generative-model output quality — that's
  `prompt-evaluation.md`; unit tests assert on deterministic domain logic, not model output.

## Location and Naming

Tests live under `tests/unit/<engine>/`, mirroring the `src/argus/<engine>/` layout
(`docs/repository-structure.md`). Test names are full sentences describing the behavior under test
(`skills/testing.md`) — `test_confidence_never_exceeds_one_regardless_of_evidence_volume`, not
`test_confidence_bounds`.

## Coverage Expectations

Coverage is a byproduct of testing every Acceptance Criterion in a spec (`.ai/coding-philosophy.md`
§8), not a target pursued for its own sake. A pure function with a business rule and no test is
incomplete work, full stop — but a coverage percentage is not, by itself, evidence the right things
were tested. Review (`prompts/review-code.md` §7) checks whether tests read as documentation of a
real rule, not whether a coverage number was hit.

## Speed

Because these tests do no I/O, the entire unit suite should run in seconds, not minutes. If it
doesn't, something has drifted from the functional-core discipline this layer depends on — treat a
slow unit test as a design smell worth investigating, not just a nuisance to parallelize away.
