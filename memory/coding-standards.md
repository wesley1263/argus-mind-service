# Coding Standards

## Toolchain

Python `>=3.14`, Poetry-managed (never bare `pip install`). Ruff for lint+format. **mypy in strict
mode** — an untyped function fails review. pytest for tests. Everything runs through Docker
Compose (`docker compose run --rm lint` / `test`), never the host directly.

## Clean Architecture (per Engine)

```
domain/ ← application/ ← ports/ ← adapters/
```

Dependencies point inward only.

- **`domain/`** — the Engine's own model + pure business rules. No framework, no I/O, no `async`.
  Immutable, typed structures (`@dataclass(frozen=True)`), never untyped dicts.
- **`application/`** — use cases orchestrating the domain against **ports** (interfaces), still no
  framework import. If a use case accepts a FastAPI `Request` or an ORM session "just to pass
  through," that's a layering violation — fix it.
- **`ports/`** — interfaces shaped around what `application/` needs, not what an external system
  happens to offer. Includes the Engine's *published contract* (the only thing another Engine may
  import from this package).
- **`adapters/`** — concrete implementations: Postgres repository, HTTP router, external clients.
  `async def` lives here (and only here) — real I/O is the only reason to be async.

Ask, when unsure where code belongs: *does this know Postgres/FastAPI/a specific vendor exists?*
Yes → adapter. No → domain/application.

## Domain-Driven Design

- Bounded contexts = the five Engines. Ubiquitous language = `memory/glossary.md`, used identically
  in code, tests, comments, and conversation.
- Entities have identity + a lifecycle (`Student`, `Learning Node`, `Session`, `Document`). Value
  objects are immutable and compared by value, no identity (`Confidence`, `Knowledge Edge`).
- Aggregate boundary = "what must be transactionally consistent together at write time" — not
  "everything conceptually related."

## Postgres

- Schema-per-Engine (`ingestion`, `knowledge`, `evidence`, `learning_state`, `generation`,
  `platform`). **No cross-schema foreign keys** — cross-Engine references are plain UUID columns,
  no `REFERENCES` constraint (a FK there would be a hidden coupling bypassing the contract rule).
- Immutability a spec declares (e.g. Evidence, append-only) is enforced by revoking `UPDATE`/
  `DELETE` grants at the DB level, not just by "we won't call it in code."
- SQL/ORM queries live in `adapters/`, behind a repository-shaped port. Never in `application/`.

## Docker

Multi-stage Dockerfile (`builder` installs deps via Poetry; `runtime` copies only what's needed).
Minimum Compose services: `db`, `migrate`, `app`, `lint`, `test`. `lint`/`test` are one-shot
(`run --rm`) and reuse the `builder` stage. Deps installed from the committed lockfile, never
resolved fresh at build time.

## Errors

Each Engine has its own small exception hierarchy rooted in one base (e.g. `IngestionError`), named
after the business condition, not the Python mechanics. `adapters/` translate typed exceptions to
the boundary response (HTTP status, etc.) in one place, not scattered `try/except`. Never catch a
bare `Exception` to keep things running — an unexpected failure should be loud.

## Testing

- **Unit** (`tests/unit/<engine>/`) — `domain/`/`application/` only, no I/O, no mocks. If a test
  needs a mock, I/O has leaked into the core; fix the code, not the test.
- **Integration** (`tests/integration/<engine>/`) — adapters against the real containerized DB.
  Contract tests (producer + real consumer code, no mocks) live here too.
- **Architecture** (`tests/architecture/`) — mechanically enforced structural rules: no cross-Engine
  imports except via a published contract; `Chunk` never appears outside Ingestion Engine; no
  framework imports in `domain/`/`application/`.
- A bug fix always starts with a failing test reproducing it (that test becomes the regression test
  — there's no separate "regression suite").

## Naming

Every name representing a business concept matches `memory/glossary.md` exactly
(`LearningNode` in code, "Learning Node" in prose — same entity). Infrastructure names (a retry
helper, a connection wrapper) are exempt.

## Don't

- Add abstraction, config flags, or generality for a case nobody's asked for yet.
- Write comments explaining *what* code does — name things well instead. Only comment a genuine
  non-obvious *why*.
- Let a "quick fix" cross an Engine boundary or bypass Validation "just this once."
