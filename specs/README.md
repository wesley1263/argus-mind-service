# Specifications

One spec per feature, numbered in build order. Every non-trivial change starts as a spec here
before any code is written.

- **[`000-template.md`](000-template.md)** — copy this to start a new spec. Every spec covers
  Business Context, Goals, Requirements, Acceptance Criteria, Sequence Diagram, State Diagram, API,
  Events, Database, Risks, Future Work, and Definition of Done.

## Specs

| Spec | Scope |
|---|---|
| [`001-document-ingestion.md`](001-document-ingestion.md) | Ingestion Engine |
| [`002-knowledge-engine.md`](002-knowledge-engine.md) | Knowledge Engine |
| [`003-evidence-engine.md`](003-evidence-engine.md) | Evidence Engine |
| [`004-learning-state-engine.md`](004-learning-state-engine.md) | Learning State Engine |
| [`005-generation-engine.md`](005-generation-engine.md) | Generation Engine |
| [`006-study-session.md`](006-study-session.md) | Cross-Engine — the Adaptive Loop end to end |
| [`007-student-progress.md`](007-student-progress.md) | Cross-Engine, read-only progress view |
| [`008-authentication.md`](008-authentication.md) | Platform — identity, not one of the five Engines |

## Writing a New Spec

1. Copy `000-template.md` to `specs/NNN-kebab-case-name.md` (next sequential number).
2. Use only terms already in `memory/glossary.md` — add one there in the same change if a term is
   missing.
3. Decide whether the spec needs an ADR (see `CLAUDE.md`'s Architecture section) before planning
   implementation.
4. From there: Plan (`architect` subagent) → Implement (`backend`) → Review/Test
   (`reviewer`/`tester`) → Docs (`docs`) → Merge.
