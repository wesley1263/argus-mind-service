# Skill: Architecture (Applied)

`.ai/architecture.md` is the canonical architecture reference — read it first. This skill is the
practical checklist for applying it while actually writing a change, so the rules there turn into
habits rather than something re-read from scratch every time.

## Before Writing Any Code, Ask

1. **Which Engine owns this concept?** Check the ownership table in `.ai/architecture.md` §2. If
   the answer isn't obvious, that's a sign the concept — or the boundary — needs to be clarified in
   a spec before code is written, not guessed at in the implementation.
2. **Am I about to cross an Engine boundary?** If the code you're writing needs data or behavior
   from another Engine, it must go through that Engine's published contract
   (`Engine Contracts`/`Shared Kernel` in `.ai/architecture.md` §4) — never an import of that
   Engine's package. If no contract exists yet for what you need, that's a spec-and-possibly-ADR
   task, not a quick import.
3. **Is this decision reversible?** If reversing it later would break a contract, require a data
   migration with no rollback, or rename a glossary term, it needs an ADR before implementation
   (`.ai/development-principles.md` §2) — see `adr/README.md`.
4. **Does this belong in the functional core or at the edge?** Business rules go in `domain/`/
   `application/` as pure functions; I/O goes in `adapters/`. See `skills/clean-architecture.md`.

## Reading the Two Architecture Diagrams Correctly

`.ai/architecture.md` has two diagrams that are easy to conflate:

- The **runtime data flow** diagram (§3) shows how data actually moves when the system is running —
  Document through Ingestion, Knowledge, Evidence, Learning State, Generation, Validation. This is
  useful for understanding *what happens*.
- The **module relationships** diagram (§4) shows what code is allowed to *import*. This is useful
  for understanding *what you're allowed to write*. Runtime data flowing from Knowledge Engine to
  Generation Engine does **not** mean Generation Engine's code may import Knowledge Engine's
  package — it means Generation Engine calls Knowledge Engine's published contract.

Confusing the two is the most common way a boundary violation sneaks into a change that looks
correct at first glance.

## Recognizing an Emerging Boundary Violation

Warning signs to stop and reconsider, even if the code compiles and tests pass:

- A single change touches `domain/` or `adapters/` internals of two different Engines.
- A type from one Engine's internal model shows up as a parameter or return type in another
  Engine's code, without having gone through a published contract type.
- Ingestion Engine's Chunk (or any future Engine's declared internal-only concept) appears anywhere
  outside its owning package.
- A "just this once, it's faster" direct database query reaches into another Engine's schema
  (`skills/postgres.md`).

## When the Architecture Itself Needs to Change

Proposing a new Engine, splitting one, merging two, or changing what a contract publishes is always
an ADR-level decision (`.ai/development-principles.md` §8, §4) — use
`prompts/review-architecture.md` and `prompts/create-adr.md` to work through it before touching
code.
