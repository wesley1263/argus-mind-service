# Specifications

This folder holds feature specifications — the artifact that opens the Spec-Driven Development
lifecycle described in `/.ai/development-principles.md`. Every non-trivial change to this
repository starts as a spec here before any code is written.

- **[`template.md`](template.md)** — copy this to start a new spec. Every spec must cover Business
  Context, Goals, Requirements, Acceptance Criteria, Sequence Diagram, State Diagram, API, Events,
  Database, Risks, Future Work, and Definition of Done.

## Example Specifications

These are worked examples showing the template applied to the platform's actual domain — one per
Engine, one Platform-level capability, and two cross-Engine orchestrations that show how a
Student-facing feature composes Engine contracts without owning their internals.

| Spec | Scope |
|---|---|
| [`authentication.md`](authentication.md) | Platform — identity, not one of the five Engines. |
| [`document-ingestion.md`](document-ingestion.md) | Ingestion Engine |
| [`knowledge-engine.md`](knowledge-engine.md) | Knowledge Engine |
| [`evidence-engine.md`](evidence-engine.md) | Evidence Engine |
| [`learning-state-engine.md`](learning-state-engine.md) | Learning State Engine |
| [`generation-engine.md`](generation-engine.md) | Generation Engine |
| [`study-session.md`](study-session.md) | Cross-Engine — the Adaptive Loop end to end. |
| [`student-progress.md`](student-progress.md) | Cross-Engine, read-only — a progress view over Learning State and the Knowledge Graph. |

## Writing a New Spec

1. Copy `template.md` to `specs/<kebab-case-name>.md`.
2. Use only terms already defined in `/glossary/README.md`; add a term there in the same change if
   one is missing (`.ai/constitution.md` Article VI).
3. Decide whether the spec needs an ADR (`.ai/development-principles.md` §2) before planning
   implementation.
4. Follow `docs/development-workflow.md` from here: Plan → Implement → Definition of Done →
   Review → Merge.

The [`prompts/generate-specification.md`](../prompts/generate-specification.md) prompt automates
starting a new spec from this template with the right context loaded.
