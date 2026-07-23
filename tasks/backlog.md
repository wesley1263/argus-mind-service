# Backlog

Sprint sequence for `argus-mind-service`. A sprint is "done" when its spec's Acceptance Criteria
all pass against real code — not when it's merely planned.

| Sprint | Scope | Status |
|---|---|---|
| 0 | Foundation (this repo's governance/spec layer) | Done |
| 1 | Document Ingestion (`specs/001-document-ingestion.md`) | Planned — see `tasks/sprint-001.md` |
| 2 | Knowledge Engine (`specs/002-knowledge-engine.md`) | Not started |
| 3 | Evidence Engine (`specs/003-evidence-engine.md`) | Not started |
| 4 | Learning State Engine (`specs/004-learning-state-engine.md`) | Not started |
| 5 | Generation Engine (`specs/005-generation-engine.md`) | Not started |
| 6 | Study Session (orchestration, `specs/006-study-session.md`) | Not started |
| 7 | Student Progress (`specs/007-student-progress.md`) | Not started |
| 8 | Authentication (`specs/008-authentication.md`) | Not started |
| 9 | Flutter MVP | Not started — no backend work in this repo |

**Note on a previous version of this backlog:** an earlier draft marked "Embedding Engine" and
"Retrieval Engine" as separate done sprints. Neither has a spec, an ADR, or any supporting doc, and
neither fits the five-Engine architecture (`Embedding` is Ingestion Engine's internal concept, not
its own Engine — see `adr/ADR-002-chunk-internal-only.md`'s reasoning, which applies the same way).
Dropped from this backlog until there's an actual spec and ADR-level justification for a new Engine
boundary.

## Adding a Sprint

A new sprint needs a spec (`specs/NNN-*.md`) before it gets a task breakdown. If it implies a new
Engine or a new cross-Engine contract, it needs an ADR too — see `CLAUDE.md`'s Architecture section
for when.
