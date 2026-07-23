# API Conventions

## FastAPI Is an Adapter

FastAPI lives entirely in `adapters/http/`, never imported from `domain/`/`application/`. One
`APIRouter` per Engine, versioned/Engine-scoped prefix (e.g. `/v1/documents` for Ingestion Engine).

## Route Handlers Are Thin

Parse/validate (Pydantic) → call one `application/` use case → map result to a response. No
business logic in the handler. Two model families, never merged:

- **Pydantic wire models** (`adapters/http/schemas.py`) — the request/response shape.
- **Domain models** (`domain/`) — the business concept, with no idea HTTP exists.

The handler converts between them explicitly.

## Errors

Typed domain/application exceptions map to HTTP status via exception handlers registered once per
Engine's router (`adapters/http/exception_handlers.py`) — not scattered `try/except` per route.

## Async & Startup

Routes and adapters are `async def` where they do real I/O. Use FastAPI's `lifespan` context
manager (`@asynccontextmanager`, passed as `FastAPI(lifespan=lifespan)`) for startup/shutdown
(DB connections, DI container wiring) — not the deprecated `@app.on_event` hooks.

## Dependency Injection

Use a DI container (`platform/container.py`) to wire adapters into use cases; routes receive a
fully-composed use case via `Depends(Provide[Container....])`. Never construct dependencies by hand
inside a route module. Unit tests bypass the container entirely — construct the use case directly
against in-memory fakes.

## Contracts

OpenAPI is a contract — a client (including a future mobile client) generates its bindings from it.
Changing a response shape without updating the corresponding spec's API section is a breaking
change (needs an ADR if it breaks an existing consumer). An Engine's published event/DTO types live
in its own `ports/` module — the one thing another Engine may import from that package.

## Status Vocabulary

Keep a spec's internal state-machine states (e.g. `Uploaded/Parsing/Structuring/Ready/Failed`)
distinct from the public API's `status` field if the public field is meant to be simpler — define
the mapping explicitly in one place (`adapters/http/mappers.py`), not ad hoc per endpoint.

## Auth

No Operator/admin role exists yet — endpoints that aren't Student-facing are unauthenticated until
a role is specced. When Student-facing, use the pattern in `specs/008-authentication.md` (Access
Token short-lived, Refresh Token rotated, never trust client-reported identity).
