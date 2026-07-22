# Skill: Python

How Python is written in `argus-mind-service`. This is the base skill every other backend skill
(`fastapi.md`, `postgres.md`, `clean-architecture.md`) assumes. Governed by
`/.ai/coding-philosophy.md`; this document makes that philosophy concrete in Python terms.

## Toolchain

- **Python `>=3.14`**, dependencies managed with **Poetry** (`pyproject.toml`). Never `pip install`
  directly into an environment outside Poetry.
- **Ruff** for both linting and formatting (replaces separate flake8/black/isort). One config, one
  tool, no debate about style.
- **mypy in strict mode** for the whole codebase. A function without full type annotations does not
  pass review. `Any` is a deliberate, commented escape hatch, not a default.
- **pytest** for tests. See `skills/testing.md` for how tests are structured.
- All of the above run through Docker Compose, never the host directly — see `skills/docker.md` and
  the root `CLAUDE.md`.

## Typing Is Not Optional

Every public function, method, and module-level value is fully typed. Domain models
(`domain/` — see `skills/clean-architecture.md`) are expressed as `@dataclass(frozen=True)` or
equivalent immutable, typed structures — never as untyped dicts passed around by convention. If a
concept has a shape, that shape is a type, not a comment.

## Functional Core, in Python Terms

Per `.ai/coding-philosophy.md` §3, business logic is pure functions with no I/O. In this codebase
that means:

- `domain/` and most of `application/` contain **synchronous, non-`async` functions**. There is no
  I/O to await, so there is nothing to make async.
- `async def` appears only in `adapters/` (HTTP handlers, DB calls, external API calls) — anywhere
  actual I/O happens. If a function you're writing in `domain/` seems to need `async`, that's a
  sign I/O has leaked into the core; push it to an adapter and pass the result in as a plain value.
- Pure functions in `domain/` take and return immutable values. They never mutate an argument in
  place, log, read the clock, or touch randomness directly — timestamps and random values are
  generated at the edge and passed in.

## Errors

- Each Engine defines its own small exception hierarchy rooted in one base exception
  (e.g., `IngestionError`, `ValidationFailed` for Generation Engine's Validation outcome — see
  `specs/generation-engine.md`). Exceptions are named after what went wrong in business terms, not
  after the Python mechanics.
- Domain code raises typed exceptions for conditions that are part of the business rule (a
  malformed Knowledge Edge, an over-limit Document). It does not raise for conditions the type
  system already rules out.
- `adapters/` code translates typed domain exceptions into the appropriate boundary response (an
  HTTP status in a FastAPI adapter, a retried message in a queue adapter) in one place — not
  scattered `try/except` at every call site. See `skills/fastapi.md`.
- Never catch a bare `Exception` to "keep things running." An unexpected failure should be loud,
  per `.ai/coding-philosophy.md` §7.

## Naming

Every name that represents a business concept matches `/glossary/README.md` exactly — `LearningNode`,
not `Node` or `KNode`; `confidence`, not `score` or `mastery_level` (see the glossary's explicit
note that "Mastery" is informal-only). Technical/infrastructure names (a retry decorator, a
connection-pool wrapper) use ordinary Python conventions and are exempt.

## What a Good Module Looks Like

A `domain/` module in this codebase has: typed, immutable data structures; pure functions operating
on them; no imports from `adapters/`, no framework imports, no other Engine's package (see
`.ai/architecture.md` §4). If you can't explain what a function does without mentioning a database,
an HTTP request, or another Engine by name, it belongs in `domain/`. If you can't explain it without
mentioning those things, it belongs in `adapters/`.
