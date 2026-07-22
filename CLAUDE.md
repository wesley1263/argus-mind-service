# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Status

`argus-mind-service` is the adaptive-learning core of Smart App. Three foundation phases are
complete — governance (`.ai/`, `docs/`, `glossary/`, `adr/`), reusable development assets (`specs/`,
`skills/`, `prompts/`, `templates/`, `testing/`), and the automation layer's specification
(`automation/`, `.github/`). There is still **no application code, no `src/`, no `tests/`, no
Docker/Compose setup, and no `CHANGELOG.md` yet** — everything so far is markdown, including the CI
workflows under `automation/github-actions/` and `automation/workflows/`, which are precise
specifications, not runnable `.yml` files (`automation/README.md` explains why). The first
implementation task is also the task that must create the actual `docker-compose.yml` (with `lint`
and `test` services, per `skills/docker.md`), the `src/` and `tests/` trees, the real
`.github/workflows/*.yml` translated from `automation/github-actions/*.md`, and an initial
`CHANGELOG.md` in Keep a Changelog format — do not assume any of these already exist without
checking.

## Commands

- Dependency management: `poetry install` (Python `>=3.14`, no dependencies declared yet — see
  `pyproject.toml`).
- **Absolute rule: every command execution (build, lint, test) goes through Docker Compose, never
  the local host.** Once the Docker setup exists:
  - Full test suite: `docker compose run --rm test`
  - Lint/pre-commit checks: `docker compose run --rm lint`
- There is no single-test command yet because no test framework or `tests/` tree exists yet; wire
  one up (e.g. a `test` service invoking the project's test runner with a path/marker argument)
  when the first tests are added, following `docs/repository-structure.md`'s planned
  `tests/{unit,integration,contract}` layout.

## Mandatory Development Workflow

Every change follows this exact sequence — do not skip or reorder steps:

1. **Analyze** — read the relevant existing code/docs before proposing any change.
2. **Branch** — create an appropriately named branch. **Never commit without explicit user
   request.** Prefixes: `feat/`, `test/`, `refact/`, `bugfix/`, `hotfix/`, `fix/`.
3. **Red** — write the test first, expecting it to fail.
4. **Green** — write the minimum code needed to make the test pass.
5. **Pre-commit** — run `docker compose run --rm lint` and fix everything it reports.
6. **Changelog** — update `CHANGELOG.md` at the end (Keep a Changelog format).

This lifecycle is layered on top of, not a replacement for, the repo's own Spec-Driven Development
process (see Architecture below): Spec → (ADR, if irreversible/cross-Engine) → Plan → Implement
(red/green) → Definition of Done → Review → Merge.

## Architecture

The platform's domain model and process rules are defined in `.ai/` and are **binding** — if
`docs/` (the human-readable mirror) ever disagrees with `.ai/`, `.ai/` wins. Read
`.ai/constitution.md` before making any non-trivial change; it is the highest-authority document
in the repo.

**Five Engines, sovereign boundaries.** The system is a modular monolith decomposed into five
Engines, each owning its own domain model and exposing only a narrow published contract
(`.ai/architecture.md` §2, ADR-001):

| Engine | Owns (private) | Publishes |
|---|---|---|
| Ingestion Engine | Chunk, chunking strategy | Document, Chapter, Topic |
| Knowledge Engine | extraction/graph-building strategy | Knowledge Graph, Learning Node, Knowledge Edge |
| Evidence Engine | Session transport/collection details | Evidence, Session |
| Learning State Engine | confidence model/algorithm | Learning State, Confidence |
| Generation Engine | generation technique | Generation Task, Validation outcome |

An Engine must never import another Engine's package directly — cross-Engine access only happens
through `Shared Kernel` (pure ubiquitous-language value types) and published `Engine Contracts`
(`.ai/architecture.md` §4). This is the single most important rule to check before adding any
cross-Engine dependency, and it is enforced in review regardless of convenience.

**Runtime data flow is a closed loop, not a pipeline**: Document → Ingestion → Knowledge (builds
Knowledge Graph) → [Student Session → Evidence → Learning State (Confidence)] → Generation Task →
Validation → back to Session. See `.ai/architecture.md` §3 for the full diagram.

**Non-negotiable invariants** (violating these is a Constitutional issue, not a style nit —
`.ai/constitution.md`, enforced via `.ai/review-checklist.md`):

- **Evidence is truth; Learning State/Confidence are always derived**, never written directly by
  any code path, including tooling/migrations (Article IV, ADR-003, ADR-004). Corrections are new
  Evidence records, never edits to existing ones.
- **Nothing reaches a Student without passing Validation** (Article V, ADR-005). A Validation
  failure must produce a fallback/retry, never a silent bypass.
- **Chunk never crosses the Ingestion Engine boundary** (ADR-002) — it must not appear in any
  contract, API, or Student-facing surface.
- **Ubiquitous language is law** (Article VI): every business-concept name in code, tests,
  comments, and logs must exactly match its definition in `glossary/README.md`. A new business
  concept gets a glossary entry in the same change that first uses it.
- **No speculative abstraction** (Article VIII): build exactly what the current Spec requires.

**Irreversible or cross-Engine decisions require an ADR** before implementation (Article VII,
`.ai/development-principles.md` §2) — see `adr/README.md` for the index and `adr/template.md` for
the format. Existing ADRs (`adr/ADR-001`…`ADR-005`) record the reasoning behind the boundaries
above and are useful precedent when a new decision looks similar to one already made.

**Planned code layout** (not yet materialized — see `docs/repository-structure.md` for the full
tree): `src/argus/{ingestion_engine,knowledge_engine,evidence_engine,learning_state_engine,
generation_engine,shared_kernel,platform}`, where each Engine package follows a uniform
`domain/ → application/ → ports/ → adapters/` (hexagonal) shape defined in `.ai/architecture.md` §5
and `.ai/coding-philosophy.md`.

Key reference docs, in the order a new task typically needs them: `.ai/constitution.md` →
`.ai/architecture.md` → `glossary/README.md` → relevant `adr/ADR-*.md` →
`.ai/coding-philosophy.md` → `.ai/definition-of-done.md` / `.ai/review-checklist.md`.

## Reusable Development Assets (Phase 2)

Before implementing anything, check whether a reusable asset already covers it — these exist
specifically so an implementation task doesn't re-derive conventions from scratch:

- `specs/` — feature specifications (`specs/template.md`'s shape: Business Context, Goals,
  Requirements, Acceptance Criteria, Sequence/State Diagrams, API, Events, Database, Risks, Future
  Work, Definition of Done). Eight worked examples already exist, one per Engine plus Authentication,
  Study Session, and Student Progress.
- `skills/` — per-technology conventions (`python.md`, `fastapi.md`, `postgres.md`, `docker.md`,
  `github-actions.md`, `clean-architecture.md`, `ddd.md`, `testing.md`, `prompt-engineering.md`,
  `architecture.md`, `flutter.md`). Read the relevant one before writing code in that area.
- `prompts/` — ready-to-use prompts for common tasks (`implement-feature.md`,
  `generate-specification.md`, `review-code.md`, `create-adr.md`, and others) — each states exactly
  which docs to load as context.
- `templates/` — lightweight templates for planning artifacts (feature, epic, RFC, spike, bug, task,
  decision, release, meeting notes); see `templates/README.md` for how to choose between
  `decision.md`, `rfc.md`, and a full ADR.
- `testing/` — the testing strategy: unit, integration (including contract tests), architecture
  tests, golden dataset, prompt evaluation, regression. `testing/architecture-testing.md`
  specifically enumerates every rule that must be mechanically enforced, not just reviewed by eye.

## Automation Layer (Phase 3)

`automation/` specifies — but does not yet implement — the CI/CD and quality-gate layer:

- `automation/github-actions/` — 15 required/advisory CI workflow specs (lint, tests, coverage,
  docker build, dependency/secret scanning, architecture/spec validation, spec drift, dead code,
  unused dependencies, versioning, release, deploy, documentation).
- `automation/quality-gates.md` — the actual numeric thresholds (coverage floors, complexity limits,
  etc.) those workflows enforce.
- `automation/ai-automation/` — prompt evaluation, golden dataset validation, LLM cost monitoring,
  prompt regression, and Knowledge Graph validation workflows.
- `automation/cicd/` — environment topology (Local/Development/Staging/Production), Blue-Green
  deploys, rollback (which never touches Evidence/Learning State — ADR-003, ADR-004), and the
  versioning/tag scheme.
- `automation/documentation-automation/` — workflows that keep `adr/README.md`, `glossary/README.md`,
  and `specs/README.md`'s index tables mechanically in sync with their source files.
- `.github/PULL_REQUEST_TEMPLATE.md` and `.github/ISSUE_TEMPLATE/*.md` **are** real, functional
  GitHub artifacts already (unlike the workflow specs, a template's markdown *is* its
  implementation).

When implementation begins, translating an `automation/github-actions/*.md` spec into a real
`.github/workflows/*.yml` file is a mechanical task — the spec already states trigger, steps, gate,
secrets, and failure behavior precisely enough to implement from directly.
