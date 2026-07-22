# Skills

A skill teaches a future AI agent (or engineer) exactly how development should happen for a
specific technology or discipline in this project — not a generic tutorial, but this repository's
own conventions, each tied back to `.ai/`, `glossary/`, and `adr/`.

## How to Use These

Read the skill for whatever you're about to touch before writing code. Skills assume the rules in
`.ai/constitution.md`, `.ai/architecture.md`, and `.ai/coding-philosophy.md` and make them concrete
in a specific technology; they never override those documents.

| Skill | Read this before… |
|---|---|
| [`python.md`](python.md) | Writing any backend code — the base skill every other backend skill assumes. |
| [`fastapi.md`](fastapi.md) | Adding or changing an HTTP endpoint. |
| [`postgres.md`](postgres.md) | Adding a table, column, or migration. |
| [`docker.md`](docker.md) | Running anything locally, or changing `docker-compose.yml`/Dockerfiles. |
| [`github-actions.md`](github-actions.md) | Changing CI workflows or required checks. |
| [`prompt-engineering.md`](prompt-engineering.md) | Writing or changing a Generation Engine prompt. |
| [`testing.md`](testing.md) | Writing an individual test. For overall test *strategy*, see `/testing/README.md`. |
| [`architecture.md`](architecture.md) | Deciding where new code belongs, or whether you're about to cross an Engine boundary. |
| [`clean-architecture.md`](clean-architecture.md) | Structuring code inside an Engine's `domain/application/ports/adapters` layers. |
| [`ddd.md`](ddd.md) | Modeling a new entity, value object, or aggregate boundary. |
| [`flutter.md`](flutter.md) | Building a client that consumes this backend (this repository has no Flutter code itself). |

## Relationship to Other Folders

- `.ai/` — the binding rules these skills make concrete.
- `specs/` — skills are referenced *from* specs (e.g., a spec's API section points at
  `fastapi.md`'s conventions); a skill never substitutes for writing a spec.
- `prompts/` — several reusable prompts (`prompts/implement-feature.md`,
  `prompts/generate-tests.md`) explicitly load the relevant skill(s) as context.
