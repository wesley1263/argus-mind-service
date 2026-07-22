# Prompt: Generate Tasks

**Use when:** an Approved spec needs to be broken into an ordered implementation plan (the "Plan"
step of `.ai/development-principles.md` §1) before implementation begins.
**Inputs required:** the spec's file path.
**Loads context from:** the spec, `.ai/architecture.md` §5 (Engine internal shape),
`templates/task.md`.

```
Break {{spec_path}} into an ordered list of implementation tasks.

Read the spec's Requirements, Acceptance Criteria, API, Events, and Database sections fully first.

Order tasks so each one is independently completable and, where possible, independently testable,
following the layered shape in /.ai/architecture.md §5:
1. Database/migration tasks (skills/postgres.md) — schema first, since domain code needs it to
   persist against.
2. Domain tasks — the pure business rules and types (skills/python.md, skills/clean-architecture.md
   domain/ layer), buildable and testable with no I/O before anything else exists.
3. Application tasks — use cases orchestrating the domain, against ports (interfaces) rather than
   concrete adapters.
4. Adapter tasks — concrete implementations of each port (a Postgres repository, an HTTP client, a
   FastAPI router) — these can often proceed in parallel once ports are defined.
5. Contract tasks — if this spec adds or changes a published Engine contract, a task for the
   contract test itself, covering both producer and consumer sides.
6. Integration/end-to-end task — wiring it all together and verifying the spec's Sequence Diagram
   actually happens as described.

For each task, use templates/task.md's shape: a one-sentence description, which Acceptance
Criterion/Criteria it serves, its rough size, and any task it depends on. Flag any task that would
require touching more than one Engine's internals — that's a signal the spec or the task breakdown
has a boundary problem, not something to implement as-is (skills/architecture.md).

Do not include tasks for anything not in the spec's Requirements — no "while we're at it" tasks.
```

**Output:** an ordered, dependency-aware task list, each task scoped to one layer of one Engine,
ready to drive `prompts/implement-feature.md` one task at a time.
