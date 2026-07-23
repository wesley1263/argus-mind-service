# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

See `PROJECT.md` for what this product is and why. This file is the binding rulebook for how to
work in the repo.

## Repository Status

No application code exists yet — no `src/`, no `tests/`, no `docker-compose.yml`, no
`CHANGELOG.md`. Every spec (`specs/`) and the current sprint plan (`tasks/sprint-001.md`) assume
you're building these from scratch. Don't assume any of them exist without checking.

## Commands

- `poetry install` (Python `>=3.14`; no dependencies declared yet — see `pyproject.toml`).
- **Absolute rule: every build/lint/test runs through Docker Compose, never the local host.** Once
  the Compose setup exists (`tasks/sprint-001.md` creates it): `docker compose run --rm test`,
  `docker compose run --rm lint`.

## Mandatory Workflow

1. **Start with the `architect` subagent** (`.claude/agents/architect.md`) for anything beyond a
   trivial fix — it scopes the work as a small vertical slice and says which spec/ADR/memory docs
   actually matter for it. Don't skip straight to coding.
2. **Branch** — never commit without explicit request. Prefixes: `feat/`, `test/`, `refact/`,
   `bugfix/`, `hotfix/`, `fix/`.
3. **Red** — write a failing test first.
4. **Green** — minimum code to pass it.
5. **Lint** — `docker compose run --rm lint`, fix everything.
6. **Review** — `reviewer` subagent (architecture/quality/security) then `tester` subagent
   (Acceptance Criteria, runs the suite).
7. **Docs + Changelog** — `docs` subagent syncs spec status/diagrams; update `CHANGELOG.md`
   (Keep a Changelog format).

Full lifecycle: **Spec → ADR (if irreversible/cross-Engine) → Plan → Implement (Red/Green) →
Review/Test → Docs/Changelog → Merge.**

## Subagents (`.claude/agents/`)

| Agent | Role | Writes code? |
|---|---|---|
| `architect` | Scopes the task, picks relevant docs, plans the increment, checks architecture/ADR compliance | No |
| `backend` | Implements one task (Branch → Red → Green) | Yes |
| `reviewer` | Architecture, code quality, security, and domain-rule/pedagogy compliance review | No |
| `tester` | Validates against Acceptance Criteria; runs tests | No (runs tests) |
| `docs` | Syncs spec status, diagrams, README after a change lands | Docs only |

## Architecture

**Five Engines, modular monolith** (one deployable, five internally separated domains — see
`adr/ADR-001-five-engine-architecture.md`):

| Engine | Owns (private) | Publishes |
|---|---|---|
| Ingestion | `Chunk`, chunking strategy | `Document`, `Chapter`, `Topic` |
| Knowledge | extraction/graph-building strategy | `Knowledge Graph`, `Learning Node`, `Knowledge Edge` |
| Evidence | Session transport details | `Evidence`, `Session` |
| Learning State | confidence model/algorithm | `Learning State`, `Confidence` |
| Generation | generation technique | `Generation Task`, `Validation` outcome |

**Rules:**
- An Engine never imports another Engine's package directly. Cross-Engine access only happens
  through a published contract (each Engine's own `ports/` module) or `shared_kernel` (pure
  identifier/value types, no behavior). This is the single most important rule to check before
  adding any cross-Engine dependency.
- Each Engine follows the same internal shape: `domain/` (pure rules, no I/O) → `application/`
  (use cases) → `ports/` (interfaces + published contract) → `adapters/` (Postgres, HTTP, external
  calls). Dependencies point inward only.
- Runtime flow is a loop, not a pipeline: `Document → Ingestion → Knowledge (Knowledge Graph) →
  [Session → Evidence → Learning State (Confidence)] → Generation Task → Validation → back to
  Session.`

**Non-negotiable invariants** — violating these blocks review, no exceptions:
- **Evidence is truth; Learning State/Confidence are always derived**, never written directly by
  any code path. Corrections are new Evidence records, never edits to old ones (ADR-003, ADR-004).
- **Nothing reaches a Student without passing Validation** (ADR-005). A failure produces a
  fallback/retry, never a silent bypass.
- **`Chunk` never crosses the Ingestion Engine boundary** (ADR-002) — not in any contract, API, or
  Student-facing surface.
- **Ubiquitous language is law** — every business-concept name in code/tests/logs matches
  `memory/glossary.md` exactly. A new concept gets a glossary entry in the same change that
  introduces it.
- **No speculative abstraction** — build exactly what the spec requires, nothing ahead of need.

**Irreversible or cross-Engine decisions need an ADR first** — a decision needs one if reversing it
would break a published contract, need an unsafe migration, rename a glossary term, or change which
Engine owns something. See `adr/` for the five foundational ones (ADR-001–005).

**Planned code layout** (not yet materialized): `src/argus/{ingestion_engine, knowledge_engine,
evidence_engine, learning_state_engine, generation_engine, shared_kernel, platform}`, each Engine
with `domain/application/ports/adapters/`.

## Where to Look

- `specs/NNN-*.md` — one spec per feature (Business Context, Goals, API, Events, DB, Definition of
  Done). Read the relevant one before touching that area.
- `adr/ADR-NNN-*.md` — why an irreversible decision was made.
- `tasks/backlog.md`, `tasks/sprint-001.md`, `tasks/doing.md` — what's planned, what's approved for
  now, what's in progress.
- `memory/coding-standards.md` — Python/FastAPI/Postgres/Docker/Clean Architecture conventions.
- `memory/api-conventions.md` — REST/contract conventions.
- `memory/glossary.md` — the ubiquitous language, canonical.
- `memory/domain-rules.md` — business rules (e.g. "Confidence is never binary," "the child is never
  blamed") with their engineering consequence. Check before touching Confidence, Feedback, or
  regeneration logic.
- `memory/pedagogy.md` — the learning-science basis behind the domain rules, condensed.
- `prompts/` — the few prompts actually used day to day.
