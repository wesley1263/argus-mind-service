# Prompt: Implement Feature

**Use when:** a spec in `specs/` is approved and ready to be built.
**Loads:** `CLAUDE.md`, `memory/glossary.md`, `memory/coding-standards.md`,
`memory/api-conventions.md`, the relevant `adr/`, and the spec itself.

```
Implement the feature specified in {{spec_path}}.

Before writing any code:
1. Read {{spec_path}} in full — Goals, Acceptance Criteria, API, Events, Database.
2. Read CLAUDE.md's Architecture section. Confirm which Engine(s) this spec touches and that
   nothing here crosses a boundary it shouldn't.
3. Read memory/glossary.md. Every business-concept name you introduce must match it exactly — if a
   term is missing, add it there first, in this same change.
4. Read memory/coding-standards.md and memory/api-conventions.md for the relevant layer.
5. Check whether this spec's decisions are already covered by an ADR in adr/. If implementation
   reveals an irreversible or cross-Engine decision that isn't, stop and write one first.

Follow CLAUDE.md's Mandatory Workflow in order: Branch (correct prefix, no commit without explicit
request) → Red (failing test per Acceptance Criterion) → Green (minimum code — business logic in
domain/application as pure functions, I/O in adapters/) → Lint (docker compose run --rm lint) →
Review/Test (reviewer then tester subagents) → Docs/Changelog.

Do not implement anything the spec doesn't ask for. If the spec seems to be missing something
necessary, stop and say so rather than filling the gap with an assumption.
```

**Output:** a working, tested implementation on a correctly-named branch, every Acceptance
Criterion passing, lint clean, changelog updated — ready for `prompts/review-code.md`.
