# Skill: FastAPI

How FastAPI is used in `argus-mind-service`. FastAPI is an **adapter technology** — it belongs
entirely inside each Engine's `adapters/` layer (`.ai/architecture.md` §5) and must never be
imported from `domain/` or `application/`. See `skills/clean-architecture.md` for the layering this
depends on.

## One Router per Engine

Each Engine exposes its own FastAPI `APIRouter`, mounted under a versioned, Engine-scoped prefix
(`/v1/documents` for Ingestion Engine, `/v1/sessions` for Evidence Engine, and so on — see the API
sections of the specs in `/specs/`). A route handler never reaches into another Engine's router,
database session, or internal module — if it needs data from another Engine, it calls that Engine's
published contract, exactly as any other adapter would (`.ai/architecture.md` §4).

## Route Handlers Are Thin

A route handler's job is: parse and validate the request (Pydantic does this automatically), call
one `application/` use case, map the result to a response. It contains no business logic — no
confidence math, no eligibility rules, no Validation logic. If a handler has an `if` statement
deciding something business-meaningful, that logic belongs in `application/` or `domain/`, unit
tested there without any HTTP concern involved.

```
Request → Pydantic request model → application use case → domain result → Pydantic response model
```

## Two Kinds of Models, Never Merged

- **Pydantic models** (`adapters/http/schemas.py` or similar) describe the wire format: what a
  client sends and receives. They may have HTTP-specific concerns (optional fields for partial
  updates, `Field` validators for input shape).
- **Domain models** (`domain/`, per `skills/python.md`) describe the business concept and have no
  knowledge that HTTP exists.

A handler converts between the two explicitly. Reusing a domain model as a Pydantic model (or vice
versa) is a shortcut that eventually leaks a persistence or transport detail into the domain —
don't do it, even when the shapes look identical today.

## Errors Map to HTTP in One Place

Typed domain/application exceptions (see `skills/python.md` §Errors) are translated to HTTP
responses by FastAPI exception handlers registered once per Engine's router, not by `try/except` in
every individual route. A `ValidationFailed` from Generation Engine's domain, for example, maps to
a specific HTTP response shape in exactly one handler function that every route in that Engine
shares.

## Dependency Injection

Use FastAPI's `Depends` for adapter-level concerns only: a DB session, the authenticated Student
(from `skills` around Authentication in `specs/authentication.md`), a configured client for an
external service. Never inject a "whole Engine" as an undifferentiated dependency — inject the
specific port (interface) a use case needs, so tests can substitute a fake implementation of that
port without spinning up FastAPI at all.

## OpenAPI Is a Contract, Not Just Docs

FastAPI's generated OpenAPI schema is the source of truth for any client — including a Flutter
client per `skills/flutter.md` — generating its API bindings. Changing a response shape without
updating the corresponding spec's API section (`specs/*.md`) and without considering downstream
generated clients is a breaking change and follows the contract-change rules in
`.ai/development-principles.md` §4.

## Async Boundary

Route handlers and everything in `adapters/` are `async def` where they perform I/O (DB queries,
HTTP calls to other services). They call into synchronous `domain/`/`application/` code directly —
there is nothing to await there, per `skills/python.md`'s Functional Core section.
