# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Status

`argus-mind-service` is the adaptive-learning core of Smart App. Four foundation phases are
complete — governance (`.ai/`, `docs/`, `glossary/`, `adr/`), reusable development assets (`specs/`,
`skills/`, `prompts/`, `templates/`, `testing/`), the automation layer's specification
(`automation/`, `.github/`), and the domain knowledge base (`docs/domain-principles.md`,
`docs/domain/`, `docs/pedagogy/`, `docs/domain-model/`, `docs/domain-rules/`,
`docs/prompt-engineering/`, `docs/golden-dataset/`, `docs/evaluation/`, `docs/future/`). There is
still **no application code, no `src/`, no `tests/`, no
Docker/Compose setup, and no `CHANGELOG.md` yet** — everything so far is markdown, including the CI
workflows under `automation/github-actions/` and `automation/workflows/`, which are precise
specifications, not runnable `.yml` files (`automation/README.md` explains why). The first
implementation task is also the task that must create the actual `docker-compose.yml` (with `lint`
and `test` services, per `skills/docker.md`), the `src/` and `tests/` trees, the real
`.github/workflows/*.yml` translated from `automation/github-actions/*.md`, and an initial
`CHANGELOG.md` in Keep a Changelog format — do not assume any of these already exist without
checking.

## Mandatory: Start Every Task With Project Conductor

Before any task in this repository — implementation, planning, or investigation — invoke the
`project-conductor` subagent first (`.claude/agents/project-conductor.md`). It scopes the request as
a small vertical slice, selects only the documentation/ADRs/specs actually relevant to it, and
recommends which specialist subagents (see Subagent Roster below) should handle each stage. It never
writes code and never redesigns architecture — it coordinates. Do not skip straight to
implementation even for a request that looks small; Project Conductor's increment rules (small,
testable, observable, deployable, independent, reversible) are what catch an over-scoped or
premature request before any code exists.

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

## Subagent Roster

Nine subagents live in `.claude/agents/` and form the pipeline that actually executes the workflow
above and the Spec-Driven Development lifecycle in `.ai/development-principles.md`. Each agent's own
file states its full read list, constraints, and stop conditions in detail — this table is only the
order and role, so it doesn't drift out of sync with them.

| Order | Agent | Role | Produces code? |
|---|---|---|---|
| 0 | `project-conductor` | Scopes the increment, selects context, assigns specialists (invoked first, always) | No |
| 1 | `architecture-reviewer` | Confirms architectural understanding before any planning begins | No |
| 2 | `sprint-planner` | Turns an Approved spec into a task breakdown (each task < 2 hours) | No |
| 3 | `domain-reviewer` | Checks the plan against `docs/domain/`, `docs/pedagogy/`, and Domain Rules | No |
| 4 | `backend-engineer` | Implements one sprint task — the actual Branch/Red/Green work | Yes |
| 5 | `code-reviewer` | Reviews the implementation for architecture, quality, security | No |
| 6 | `qa-engineer` | Validates against Acceptance Criteria; runs the test suite | No (runs tests) |
| 7 | `documentation-engineer` | Synchronizes docs, diagrams, and spec status after approval | Docs only |
| 8 | `release-manager` | Final merge gate: lint, test, coverage, security, changelog, version | No |

This is how the Mandatory Development Workflow's steps map to named responsible parties:
`backend-engineer` performs Branch → Red → Green; `code-reviewer` and `qa-engineer` are the review
and validation step; `documentation-engineer` and `release-manager` cover Changelog and the final
Pre-commit/merge gate. A request that looks like it can skip straight to `backend-engineer` still
starts with `project-conductor` — it decides whether the earlier stages are actually needed for a
given task's size and risk, rather than skipping them by default.

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
`.ai/coding-philosophy.md` → `.ai/definition-of-done.md` / `.ai/review-checklist.md`. For anything
touching Knowledge Engine, Learning State Engine, or Generation Engine specifically, also read
`docs/domain-principles.md` and the relevant `docs/domain/*.md` document first — see the Domain
Knowledge Base section below.

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

## Domain Knowledge Base (Phase 4)

The pedagogical and product reasoning behind every Engine — read before implementing anything in
Knowledge Engine, Learning State Engine, or Generation Engine, since their correctness is defined
partly in learning-science terms, not just engineering terms:

- `docs/domain-principles.md` — the permanent, pedagogical counterpart to `.ai/constitution.md`.
  Thirteen non-negotiable principles (e.g., "Evidence Over Assumptions," "The System Never Judges
  Learning") that a technically correct change can still violate.
- `docs/domain/` — why each Engine exists pedagogically, and the end-to-end Student journey.
- `docs/pedagogy/` — fifteen learning-science principles (Active Recall, Spaced Repetition,
  Cognitive Load Theory, etc.), each with its evidence base and concrete engineering/product/prompt
  implications. Two disambiguations worth knowing up front: `docs/pedagogy/chunking.md` (the
  cognitive principle) is **not** the same as Ingestion Engine's internal `Chunk` (ADR-002); Active
  Recall and Retrieval Practice are related but distinct (cognitive act vs. pedagogical strategy).
- `docs/domain-model/` — the full entity catalog (attributes, relationships, lifecycle), companion
  to `glossary/README.md`. This phase significantly expanded the glossary itself — check
  `glossary/README.md` for current terms (e.g., `EvidenceType`, `Difficulty`, `PromptContext`,
  `SessionPlan`, `ChapterPlan`, `MindMap`, `Summary`, `Game`, `Quiz`, `Reinforcement`,
  `AdaptiveContent`, `Learning Path`) before assuming only the five-Engine-era terms exist.
- `docs/domain-rules/README.md` — business rules (e.g., "Confidence Is Never Binary," "False
  Negatives Are Worse Than False Positives," "The Child Is Never Blamed") each with business,
  pedagogical, and engineering justification — check this before implementing anything that touches
  Confidence, Feedback, or regeneration logic.
- `docs/prompt-engineering/` — a design guide per generative prompt in the platform, plus the shared
  review checklist, versioning strategy, regression strategy, and evaluation metrics. Critically:
  `docs/prompt-engineering/learning-state-prompt.md` and `evidence-engine-prompt.md` establish that
  **no prompt ever directly sets Confidence or Evidence's judgment** — a prompt produces a
  structured signal; deterministic code (per ADR-004) converts it.
- `docs/golden-dataset/` and `docs/evaluation/` — the content/structure behind `testing/golden-dataset.md`,
  and how the *product* (not the LLM) is evaluated — see `docs/evaluation/README.md`'s explicit
  metric list before adding any new success metric.
- `docs/future/` — deliberately deferred capabilities; do not implement anything here without a spec
  first (Article VIII).
