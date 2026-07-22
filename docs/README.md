# Documentation

Human-facing documentation for Smart App's `argus-mind-service`. This folder tells the same story
as `/.ai/`, but for people orienting themselves rather than for enforcement — where the two
overlap and ever disagree, `/.ai/` is canonical (see each document's header note).

| Document | Read this to understand… |
|---|---|
| [`vision.md`](vision.md) | Why Smart App exists, what problem it solves, what it deliberately isn't. |
| [`architecture-overview.md`](architecture-overview.md) | The five Engines, how data flows through them, and why. |
| [`development-workflow.md`](development-workflow.md) | How a change moves from idea to merge. |
| [`repository-structure.md`](repository-structure.md) | What exists in this repository today, and what the planned code layout will look like. |
| [`engineering-principles.md`](engineering-principles.md) | The ten-principle distillation of "how we build here." |
| [`domain-principles.md`](domain-principles.md) | The permanent, pedagogical counterpart to `.ai/constitution.md` — thirteen non-negotiable learning principles. |

## Domain Intelligence (Phase 4)

The knowledge base behind *why* the product works, not just how it's engineered — required reading
before implementing anything in Knowledge Engine, Learning State Engine, or Generation Engine.

| Folder | Covers |
|---|---|
| [`domain/`](domain/README.md) | Pedagogical rationale for each Engine, the Core Product Thesis, and the Student journey end to end. |
| [`pedagogy/`](pedagogy/README.md) | Fifteen learning-science principles, each with evidence and engineering/product/prompt implications. |
| [`domain-model/`](domain-model/README.md) | The full entity catalog — companion to `/glossary/README.md`, with attributes and relationships. |
| [`domain-rules/README.md`](domain-rules/README.md) | Business rules with business, pedagogical, and engineering justification for each. |
| [`prompt-engineering/`](prompt-engineering/README.md) | Design guides for every generative prompt in the platform. |
| [`golden-dataset/`](golden-dataset/README.md) | The structure and evaluation methodology behind `/testing/golden-dataset.md`. |
| [`evaluation/`](evaluation/README.md) | How the *product* is measured — learning outcomes, never the LLM. |
| [`future/`](future/README.md) | Deliberately deferred capabilities and their prerequisites. |

## Related, Outside This Folder

- [`/.ai/`](../.ai) — the binding rules: constitution, architecture, coding philosophy,
  development process, review checklist, definition of done.
- [`/glossary/README.md`](../glossary/README.md) — the ubiquitous language: precise definitions of
  every business term used throughout this repository.
- [`/adr/`](../adr) — the record of why the architecture's irreversible decisions were made.
- [`/README.md`](../README.md) — the repository entry point.
