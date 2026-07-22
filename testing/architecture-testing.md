# Architecture Testing

## Scope

An architecture test asserts a structural rule holds across the **whole codebase**, mechanically —
not a single feature's behavior, and not something left to a human reviewer's memory of
`.ai/architecture.md`. If a rule in `.ai/constitution.md` or `.ai/architecture.md` can be phrased as
"this must never be true anywhere in the code," it belongs here, encoded so a violation fails CI
automatically the moment it's introduced, not whenever someone happens to notice in review.

## Why This Layer Exists

Code review (`prompts/review-code.md`) catches boundary violations too — but review is performed by
people and agents with finite attention across an increasingly large codebase. A structural rule
that's only enforced by review eventually gets missed once. An architecture test enforces it every
single time, for the life of the project, with no attention cost. This is what makes the
"Constitutional, not stylistic" stance in `.ai/review-checklist.md`'s closing note actually durable.

## Rules This Layer Must Enforce

| Rule | Source | How it's typically checked |
|---|---|---|
| No Engine package imports another Engine's package directly | `.ai/architecture.md` §4 | Import-graph analysis (e.g. `import-linter` contracts) run over `src/argus/*`. |
| `Shared Kernel` depends on nothing; Engines depend only on it and on `Engine Contracts` | `.ai/architecture.md` §4 | Same import-graph tooling, asserting the dependency direction. |
| Chunk (or any Engine's declared internal-only concept) never appears outside its owning package | ADR-002 | A scan for the type/name appearing in any published contract module or any other Engine's package. |
| No code path writes to Learning State/Confidence except the Learning State Engine's own computation | ADR-004, `specs/learning-state-engine.md` AC5 | A check that no write-capable method/endpoint accepting a Confidence value exists outside that Engine. |
| No `UPDATE`/`DELETE` capability exists on Evidence tables | ADR-003, `specs/evidence-engine.md` AC4 | Static check that the application's DB role grants exclude those verbs on the relevant tables (paired with the runtime check in `integration-testing.md`). |
| No delivery path can be reached with content lacking a passing Validation record | ADR-005, `specs/generation-engine.md` AC5 | A check that the delivery function's only inputs are types that can only be constructed after Validation passes (type-level enforcement is stronger than a runtime check alone). |
| `domain/`/`application/` code contains no framework imports (FastAPI, the DB driver, an AI vendor SDK) | `.ai/coding-philosophy.md` §3, `skills/clean-architecture.md` | Import-graph check scoped per layer within each Engine, not just between Engines. |

This table grows as new invariants are declared in specs and ADRs — a spec whose Definition of Done
says "verified by architecture test" is committing to adding a row here, not just asserting it
informally.

## What This Layer Is Not

It does not replace unit tests (behavior correctness) or integration tests (does an adapter really
work). An architecture test answers "is the shape of the system still what we said it would be,"
never "does this specific function compute the right answer."

## Location and Failure Behavior

Tests live under `tests/architecture/`, run as part of `docker compose run --rm test`
(`skills/docker.md`), and are treated as required checks with no exception path
(`skills/github-actions.md`) — exactly like the boundary/integrity sections of
`.ai/review-checklist.md`, a failure here is never merged around with a follow-up.
