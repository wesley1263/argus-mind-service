# Prompt: Implement Feature

**Use when:** a spec in `/specs/` is Approved and ready to be built.
**Inputs required:** the spec's file path.
**Loads context from:** `.ai/constitution.md`, `.ai/coding-philosophy.md`,
`.ai/development-principles.md`, `.ai/definition-of-done.md`, `glossary/README.md`, the relevant
`skills/*.md` for the Engine(s) touched, and the spec itself.

```
You are implementing the feature specified in {{spec_path}}.

Before writing any code:
1. Read {{spec_path}} in full — Goals, Requirements, Acceptance Criteria, Sequence Diagram, State
   Diagram, API, Events, Database.
2. Read /.ai/constitution.md and /.ai/architecture.md. Identify exactly which Engine(s) this spec
   touches and confirm you understand the ownership/contract boundaries involved
   (/.ai/architecture.md §2, §4).
3. Read /glossary/README.md. Every business-concept name you introduce must match it exactly. If
   the spec needs a term that isn't there, stop and add it to the glossary first, in this same
   change.
4. Read the skill file(s) relevant to what you're building (e.g. skills/fastapi.md for an endpoint,
   skills/postgres.md for a migration, skills/clean-architecture.md for where code belongs).
5. Confirm whether this spec's Related ADRs section is complete. If implementation reveals a
   decision that's irreversible or cross-Engine and isn't already covered by an ADR, stop and use
   prompts/create-adr.md before continuing.

Follow the mandatory workflow from CLAUDE.md exactly, in order:
1. Branch — create a branch named feat/{{short-feature-slug}} (or the appropriate prefix). Do not
   commit without explicit request.
2. Red — write a failing test for one Acceptance Criterion at a time, confirmed failing before any
   implementation code is written.
3. Green — write the minimum code to make that test pass. Business logic goes in domain/application
   as pure functions (skills/python.md, skills/clean-architecture.md); I/O goes in adapters/.
4. Repeat Red/Green until every Acceptance Criterion in the spec is covered and passing.
5. Pre-commit — run `docker compose run --rm lint` and fix everything reported.
6. Changelog — add an entry to CHANGELOG.md (Keep a Changelog format) describing the change from a
   user/operator perspective, not an implementation-detail perspective.

Before declaring the work done, check it against /.ai/definition-of-done.md and against this spec's
own Definition of Done section, line by line — do not summarize compliance, actually verify each
box.

Do not implement anything the spec doesn't ask for (Article VIII — no speculative generality). If
you believe the spec is missing something necessary, stop and say so rather than filling the gap
with an assumption.
```

**Output:** a working, tested implementation on a correctly-named branch, satisfying every
Acceptance Criterion in the spec, passing lint, with the changelog updated — ready for
`prompts/review-code.md` or `prompts/review-pull-request.md`.
