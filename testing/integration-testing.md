# Integration Testing

## Scope

An integration test exercises `adapters/` code against the real system it wraps: a real, running
PostgreSQL instance (containerized, per `skills/docker.md`), a real HTTP call to a test instance of
another service, a real (or realistic sandboxed) call to an external model provider where
appropriate. The point is to prove the adapter actually works, not that a mock was configured
correctly (`skills/testing.md`).

## What Belongs Here

- Repository adapters: does `PostgresKnowledgeGraphRepository` actually persist and query a
  `LearningNode` correctly, including the cycle-rejection behavior at the storage layer?
- Grant/permission behavior: does the application's database role genuinely lack `UPDATE`/`DELETE`
  on `evidence.evidence_records` when connected as that role (not as a superuser)? This overlaps
  with `architecture-testing.md` — see the note below on where each lives.
- FastAPI route wiring: does `POST /v1/documents` actually reach the right use case and return the
  right status codes and shape, end to end through the HTTP layer (`skills/fastapi.md`)?
- Migration correctness: does a generated migration (`prompts/generate-migration.md`) apply cleanly
  and produce the expected schema?

## Contract Tests Live Here Too

A contract test verifies that what one Engine publishes (`.ai/architecture.md` §2) is exactly what
another Engine's consuming code expects — run using **real implementations on both sides**, not a
mocked producer or a hand-written fixture standing in for the real contract type. For example: a
test that constructs a real `DocumentIngested` event via Ingestion Engine's actual code and feeds it
into Knowledge Engine's actual consumer, asserting the consumer handles every field Ingestion Engine
actually produces (`specs/document-ingestion.md`, `specs/knowledge-engine.md`).

Contract tests are what make additive-vs-breaking contract changes (`.ai/development-principles.md`
§4) a checkable fact instead of a judgment call: a breaking change fails the consumer's contract
test immediately, at the boundary where it actually breaks something.

## Where "Architecture Test" vs. "Integration Test" Applies

Both can touch a real database, which makes the boundary easy to blur:

- **Integration test**: "does this adapter behave correctly against the real system" — a
  functional question about one adapter.
- **Architecture test** (`architecture-testing.md`): "does the *system as a whole* still obey a
  structural rule" — e.g., "no Engine's package imports another Engine's package," checked across
  the whole codebase, not one adapter at a time.

A permission-grant check ("this role can't UPDATE this table") is functional enough to be an
integration test of that specific adapter/migration; a check that scans *every* table against the
immutability rules declared across all specs is an architecture test. When in doubt, put it in
`architecture-testing.md` if it's meant to catch a *future* violation anywhere in the codebase, not
just verify today's adapter.

## Location and Naming

Tests live under `tests/integration/<engine>/`, using real containerized dependencies started via
the `test` service in `docker-compose.yml` (`skills/docker.md`) — never a developer's local
Postgres, never a service reached over the open internet during CI.

## Isolation

Each integration test owns its own data (a fresh schema/transaction rolled back after the test, or
a uniquely-namespaced dataset) so tests can run in parallel and in any order without interfering
with each other. A test that depends on another test having run first is a bug in the test, not a
sequencing requirement to document.
