# Skill: Clean / Hexagonal Architecture

The internal shape every Engine follows, specified structurally in `.ai/architecture.md` §5. This
skill explains how to actually place code into that shape, with concrete examples drawn from this
project's own domain.

## The Four Layers, and the One Rule That Matters

```
domain/  ←  application/  ←  ports/  ←  adapters/
```

Dependencies point **inward only**. `domain/` depends on nothing else in the Engine. `application/`
depends on `domain/`. `ports/` are interfaces `application/` defines and depends on the abstraction
of, not any implementation. `adapters/` depend on `ports/` (implementing them) and on external
frameworks/libraries — and nothing outside `adapters/` is allowed to depend on a framework at all.

If you're ever unsure which layer something belongs in, ask: **does this code know that Postgres,
FastAPI, or a specific AI vendor exists?** If yes, it's an adapter. If it's a business rule with no
such knowledge, it's `domain/` or `application/`.

## Worked Example: Knowledge Engine

- **`domain/`** — `LearningNode` (immutable, typed entity with an identity), `KnowledgeEdge`, and
  the pure rule "a set of Knowledge Edges must not contain a cycle" (`specs/knowledge-engine.md`
  AC2/Risks). No database, no HTTP, no extraction-model client.
- **`application/`** — the use case `ExtractLearningNodesFromTopic(topic, extraction_port) ->
  list[LearningNode]`: orchestrates calling the extraction port and applying domain validation to
  its result. Still no framework — it depends on `ports/`, not on any concrete adapter.
- **`ports/`** — `ExtractionPort` (an interface: "given a Topic, return candidate Learning Nodes"),
  `KnowledgeGraphRepository` (an interface: "persist/query Learning Nodes and Knowledge Edges").
  These are ABCs/Protocols with no implementation.
- **`adapters/`** — a concrete `PostgresKnowledgeGraphRepository` implementing
  `KnowledgeGraphRepository` (`skills/postgres.md`), a concrete extraction-model client implementing
  `ExtractionPort`, and the FastAPI router (`skills/fastapi.md`) that calls the `application/` use
  case and maps its result to an HTTP response.

Notice: swapping the extraction technique (a different model, a rules-based extractor) means
writing a new `ExtractionPort` implementation in `adapters/` — `domain/` and `application/` never
change. That's the entire point of the layering, and it's what makes Article I of the Constitution
("the AI is a tool, not the product") actually true in code, not just in prose.

## Ports Belong to the Consumer, Not the Provider

A port (interface) is defined in terms of what `application/` needs, not in terms of what a
particular external system happens to offer. `ExtractionPort.extract(topic) -> list[CandidateNode]`
is shaped around Knowledge Engine's own domain language, even if the underlying model API returns
something completely different — the adapter's job is exactly that translation.

## Testing Follows the Layers

- `domain/` and `application/` (against fake/in-memory port implementations, not mocking
  frameworks) are tested with no I/O at all — see `skills/testing.md`.
- `adapters/` are tested against the real system they wrap (real Postgres, in a container) — see
  `testing/integration-testing.md`.

## A Layer Violation to Watch For

The most common violation in a hexagonal codebase isn't a `domain/` file importing FastAPI directly
— that's obvious in review. It's `application/` accepting a framework-specific type (a FastAPI
`Request`, a SQLAlchemy `Session`) as a parameter "just to pass through." The moment that happens,
`application/` is no longer testable without that framework, and the whole point of the layering is
gone. Pass plain domain values across the `application/`↔`adapters/` boundary, always.
