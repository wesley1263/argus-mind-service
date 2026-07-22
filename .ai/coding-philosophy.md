# Coding Philosophy

> Audience: whoever (human or AI agent) is about to write code inside an Engine.
> This document governs *how* code is written. `development-principles.md` governs the *process*
> around writing it. `architecture.md` governs *where* it lives.

This document has no code examples in a specific language because no language-level implementation
decisions have been made yet in this repository (Phase 1 is foundation only). The rules below are
stack-agnostic and bind whatever implementation language and framework are chosen later.

## 1. Names Come From the Glossary, Not From the Keyboard

Every class, function, variable, table, event, and log field that represents a business concept
must use the exact term from `/glossary/README.md` — same spelling, same casing convention applied
consistently. If you need a name and the concept isn't in the glossary, that is a stop sign: either
the concept already exists under a different name (use it) or the glossary is missing a term (add
it, in the same change, before writing the code that needs it).

Technical/infrastructure names (a retry helper, a connection pool) are not business concepts and
are exempt — use good engineering judgment there.

## 2. Each Engine Is a Bounded Context

Inside its own boundary, an Engine models the world in the terms that make sense for *its* job.
Ingestion Engine's internal representation of a Chunk does not need to mean anything to Knowledge
Engine, and must not — Chunk is explicitly internal-only (ADR-002). Do not design an Engine's
internals to be "friendly" to another Engine's future needs; design it to correctly do its own job.
Cross-Engine friendliness is the job of the published contract, not of internal models.

## 3. Functional Core, Imperative Shell

Business rules — how a Confidence is computed from Evidence, how a Learning Node relationship is
validated, how a Generation Task is selected — are written as pure, deterministic functions with
no I/O: same input, same output, every time. I/O (database calls, network calls, clock reads,
random values) lives at the edges (`adapters/`) and is passed *into* the core as data, never read
by the core itself. This makes the actual methodology — the product — trivially testable without
mocks, and swappable in its I/O without touching a single business rule.

## 4. Evidence and Every Other Fact Are Immutable

Anything recorded as having happened (Evidence, a completed Session, a delivered Generation Task)
is represented as an immutable record once written. Corrections are new records, never in-place
edits (Article IV of the Constitution, ADR-003). Mutable state in this codebase should almost
always turn out to be a *cache* or *projection* of an immutable log, never the log itself.

## 5. Contracts Are Explicit and Narrow

What one Engine exposes to the rest of the system is a small, deliberately designed type or
interface — not "whatever the internal model happens to look like, exported." Prefer a contract
with three fields the consumer actually needs over one with twenty fields because the producer had
them lying around. A wide contract is a coupling liability; every field is a promise you have to
keep.

## 6. No Premature Abstraction

Build the concrete thing the current spec asks for. Do not introduce a plugin system, a strategy
pattern, or a configuration flag for a variation nobody has asked for yet. Three call sites that
look similar are not automatically a shared abstraction — abstract only when the shared *meaning*,
not just the shared shape, is confirmed. Reversing an abstraction later is cheap; reversing a
premature one that leaked into contracts is not (see Article VIII of the Constitution).

## 7. Errors Are Either Handled or They Stop the Show

Do not swallow an error to keep code "looking clean," and do not add defensive handling for a
condition that the type system or an upstream invariant already rules out. Handle errors at the
boundary where you have enough context to do something meaningful with them (retry, fall back,
surface to a caller with an explicit typed failure). Everywhere else, let them propagate.
Validation failures in the Generation Engine are not exceptions to swallow — they are an explicit,
modeled outcome (Article V, ADR-005), not a try/catch afterthought.

## 8. Tests Are Executable Specification

A test exists to pin down a behavior described in a spec, not to hit a coverage number. Prefer
one precise test that documents a real business rule ("a Learning Node's Confidence never exceeds
1.0", "Evidence for a deleted Session is still retained") over ten tests that just re-assert
implementation details. Tests for the functional core (see §3) should not need mocks — that need
is usually a sign I/O has leaked into the core.

## 9. Small, Explicit, Boring

Prefer the boring, explicit solution a future reader (human or agent, with none of today's context)
can verify by reading it once. Cleverness that saves five lines but costs five minutes of
re-derivation on every future read is a net loss over a ten-year maintenance horizon. Optimize for
"obviously correct," not "impressively terse."

## 10. Dependency Direction Is Sacred

Code inside an Engine may depend on `Shared Kernel` and on `Engine Contracts`. It must never import
another Engine's package. See `architecture.md` §4 for the enforced module graph. A shortcut that
crosses this line to "save time" is a Constitutional violation (Article III) and is rejected in
review regardless of urgency.
