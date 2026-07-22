# ADR-001: Adopt a Five-Engine Modular Architecture Within One Deployable Service

- **Status:** Accepted
- **Date:** 2026-07-21
- **Deciders:** Founding engineering (Phase 1 — Foundation)

## Context

Smart App's methodology decomposes cleanly into five distinct responsibilities: structuring raw
source material (Ingestion), modeling what can be known (Knowledge), observing what a Student
actually did (Evidence), deriving what a Student currently knows (Learning State), and deciding and
producing what they should see next (Generation). These responsibilities have different rates of
change, different failure domains, and different owners of "truth" — but the project is pre-PMF,
has no production traffic yet, and has no evidence yet about where its actual scaling or
reliability pressure will land.

We need enough structural discipline that these five responsibilities don't collapse into one
undifferentiated codebase where, six months in, "just import the other Engine's model, it's right
there" becomes the path of least resistance. We also need to avoid paying the operational cost of
a distributed system — network partitions, independent deployment pipelines, cross-service
observability — before we have a concrete reason to.

## Decision

Model the platform as five explicitly bounded **Engines** — Ingestion, Knowledge, Evidence,
Learning State, Generation — each with its own domain model, implemented as separate top-level
packages within a single deployable service (`argus-mind-service`). Engines communicate only
through explicit, versioned contracts (published types/interfaces), never by importing another
Engine's internals. See `/.ai/architecture.md` for the full contract table and the enforced module
dependency graph.

Each Engine is designed so that, if it later needs to become an independently deployed service, the
change is an infrastructure change to how its existing contract is transported — not a redesign of
what the contract means.

## Consequences

- **Easier operations today:** one deployable, one deployment pipeline, one set of runtime
  dependencies to manage, while the product is still finding its shape.
- **Enforced boundaries prevent entanglement:** because the contract is the only legal path between
  Engines, business logic can't accidentally leak from, say, Knowledge Engine into Generation
  Engine's decision-making.
- **A real path to extraction exists later:** splitting an Engine into its own service becomes an
  infrastructure exercise, not a domain-modeling one, because the contract boundary was designed in
  from day one.
- **New obligation — this boundary must be actively defended:** nothing about a monolith's file
  layout technically stops someone from importing across an Engine boundary; the discipline has to
  be enforced in review (`/.ai/review-checklist.md` §2) and, once `src/` exists, by import-boundary
  tooling.
- **New obligation — shared fate for uptime:** because all five Engines run in one process/service,
  an outage in one can, without careful design, affect the others. Non-functional isolation (timeouts,
  bulkheads) between Engines becomes a design concern to revisit as real load appears, tracked as a
  future ADR when it becomes concrete rather than speculative.

## Alternatives Considered

- **Microservices from day one**, one deployable per Engine. Rejected: the operational cost
  (independent deploys, service discovery, distributed tracing, network-partition handling) is a
  poor trade before the domain model has been proven against real usage. This project is not
  microservices-first; it becomes microservices-*capable* by designing real contracts now.
- **No Engine boundaries — a single undifferentiated application.** Rejected: this directly
  contradicts the premise that the methodology, not any one clever piece of code, is the product
  (see `/.ai/constitution.md` Article I). Without enforced boundaries, Ingestion's internal
  chunking details or Generation's model choice tend to leak into everything around them, and the
  methodology becomes inseparable from whichever implementation detail happened to be convenient
  at the time.
