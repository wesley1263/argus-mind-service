# Prompt: Review Architecture

**Use when:** evaluating a proposed change to the system's structure — a new Engine, a contract
change, a new cross-cutting concern — before it's implemented.
**Inputs required:** a description of the proposed architectural change.
**Loads context from:** `.ai/architecture.md`, `.ai/constitution.md`, `adr/README.md` and existing
ADRs, `skills/architecture.md`.

```
Review this proposed architectural change: {{proposal_description}}.

Evaluate it against /.ai/architecture.md and /.ai/constitution.md directly:

1. Does it respect Engine sovereignty (Article III)? Does every cross-Engine interaction go through
   a published contract (Shared Kernel / Engine Contracts, .ai/architecture.md §4), with no new
   direct import between Engine packages?
2. Does it introduce, remove, or redefine what an Engine owns vs. publishes
   (.ai/architecture.md §2)? If so, this needs an ADR — check whether one already exists that
   covers it (adr/README.md) or whether prompts/create-adr.md should run before this proceeds.
3. Does it preserve the non-negotiable invariants: Evidence is truth and Confidence is derived
   (Article IV, ADR-003, ADR-004), nothing reaches a Student without Validation (Article V,
   ADR-005), Chunk (or any Engine's declared internal-only concept) stays internal (ADR-002)?
4. Is it the simplest structure that satisfies the actual requirement, or does it add generality
   for a hypothetical future case (Article VIII)?
5. If it proposes a new Engine or splits/merges an existing one: does it justify the new boundary
   in terms of a distinct owned business concept and a distinct contract, the way ADR-001
   justified the original five (.ai/development-principles.md §8)?
6. Does every existing consumer of any contract this touches keep working, or is the change
   additive-only? If breaking, what is the migration path for consumers
   (.ai/development-principles.md §4)?

For each point, give a specific verdict (compliant / violation / needs an ADR) with the reasoning,
not just a summary judgment. If the proposal is sound but no ADR exists yet to record it, say so
explicitly and recommend prompts/create-adr.md as the next step before implementation.
```

**Output:** a point-by-point architectural verdict, with a clear recommendation: proceed, proceed
after writing an ADR, or reject with the specific principle it violates.
