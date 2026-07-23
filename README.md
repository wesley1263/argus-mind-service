# argus-mind-service

The adaptive-learning backend for Smart App. See `PROJECT.md` for what it is and why.

## Status

No application code yet — this repo currently holds only the specs, decisions, and conventions
needed to build it. See `tasks/backlog.md` for what's planned and `tasks/sprint-001.md` for the
approved plan for the first sprint (Document Ingestion).

## Repository Layout

```
argus-mind-service/
├── CLAUDE.md              Binding rules for anyone (human or agent) working in this repo.
├── PROJECT.md              What Smart App is, the five Engines, the product principles.
├── README.md               This file.
├── .claude/agents/         Subagent roster: architect, backend, reviewer, tester, docs.
├── specs/                  One spec per feature (NNN-name.md).
├── adr/                    Why an irreversible/cross-Engine decision was made.
├── tasks/                  backlog.md, sprint-001.md, doing.md.
├── memory/                 Coding standards, API conventions, glossary, domain rules, pedagogy.
├── prompts/                implement-feature.md, review-code.md, generate-tests.md.
├── .github/                PR and issue templates.
└── src/                    (planned) Application code.
```

## Getting Started

- **Prerequisites:** Python `>=3.14`, [Poetry](https://python-poetry.org/).
- **Setup:** `poetry install`

There's nothing to run yet — no dependencies are declared and no application code exists (see
Status above). Once `tasks/sprint-001.md` lands, this section gets real run/test commands; until
then, `CLAUDE.md`'s Commands section is the source of truth for what's available.

## Contributing

Read `CLAUDE.md` before making any change — it has the mandatory workflow, the subagent roster, and
the architecture rules that bind every change. Start with the `architect` subagent for anything
beyond a trivial fix.
