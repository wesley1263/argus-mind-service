# Prompts

Reusable, ready-to-use prompts for common tasks in this repository. Each one is self-contained: it
tells whoever runs it (human or AI agent) exactly which documents to load as context and exactly
what to check, so the same task produces consistent results regardless of who — or which
session — runs it.

## Index

| Prompt | Use for |
|---|---|
| [`implement-feature.md`](implement-feature.md) | Building an Approved spec, following the mandatory Red/Green/lint/changelog workflow. |
| [`generate-specification.md`](generate-specification.md) | Starting a new feature — produces a `specs/` file from `specs/template.md`. |
| [`generate-tasks.md`](generate-tasks.md) | Breaking an Approved spec into an ordered implementation plan. |
| [`generate-tests.md`](generate-tests.md) | Turning Acceptance Criteria into failing tests, or backfilling coverage. |
| [`generate-migration.md`](generate-migration.md) | Producing a schema migration from a spec's Database section. |
| [`refactor.md`](refactor.md) | Improving code structure without changing behavior. |
| [`review-code.md`](review-code.md) | Reviewing a diff against `.ai/review-checklist.md`. |
| [`review-pull-request.md`](review-pull-request.md) | Full PR merge-readiness: process, CI, code, Definition of Done, changelog. |
| [`review-architecture.md`](review-architecture.md) | Evaluating a proposed structural change before implementation. |
| [`create-adr.md`](create-adr.md) | Recording an irreversible or cross-Engine decision. |
| [`security-review.md`](security-review.md) | Reviewing auth, untrusted input, and data-integrity-sensitive changes. |
| [`performance-review.md`](performance-review.md) | Reviewing a change against a spec's latency/throughput requirements. |

## How These Fit the Development Workflow

Most features move through these prompts in roughly this order:

```
generate-specification → create-adr (if needed) → generate-tasks → implement-feature
    → generate-tests (as part of implement-feature's Red step) → review-code / review-pull-request
    → security-review / performance-review (as warranted by the change)
```

See `docs/development-workflow.md` for the underlying lifecycle these prompts automate, and
`skills/` for the technology-specific conventions each prompt assumes.
