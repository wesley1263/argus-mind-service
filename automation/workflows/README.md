# Automation (Broader Workflow Catalog)

This folder covers the Phase 3 "Automation" deliverable list — work that's scheduled, bot-driven,
or triggered by state changes (a spec becoming Approved, a merge to `main`) rather than strictly
gating a pull request the way `automation/github-actions/` does.

Two items from that original list — **Spec Validation** and **Architecture Validation** — are not
duplicated here: they're PR-gating checks and already fully specified as
[`github-actions/specification-validation.md`](../github-actions/specification-validation.md) and
[`github-actions/architecture-validation.md`](../github-actions/architecture-validation.md).

## Index

| Workflow | Trigger | Purpose |
|---|---|---|
| [`automatic-task-generation.md`](automatic-task-generation.md) | Spec → Approved | Runs `prompts/generate-tasks.md` automatically. |
| [`release-notes.md`](release-notes.md) | Release time | Narrative release notes from changelog data. |
| [`changelog.md`](changelog.md) | Every merge to `main` | Keeps `CHANGELOG.md` current from PR metadata. |
| [`documentation-generation.md`](documentation-generation.md) | Source changes | Regenerates derived docs (API reference) from code. |
| [`dependency-updates.md`](dependency-updates.md) | Weekly | Opens dependency-upgrade PRs. |
| [`code-metrics.md`](code-metrics.md) | Every merge / every PR | Tracks complexity, size, and quality trends. |
| [`repository-health.md`](repository-health.md) | Weekly | Rolls every other workflow's output into one digest. |

## Relationship to Other `automation/` Folders

- `github-actions/` — PR-gating checks.
- `quality-gates.md` — the specific thresholds `code-metrics.md` measures against.
- `ai-automation/` — the LLM/prompt-specific equivalent of this catalog.
- `cicd/` — deployment strategy, which `release.md`/`deploy.md` (in `github-actions/`) execute.
- `documentation-automation/` — self-updating *index* pages (ADR index, glossary index, etc.),
  distinct from `documentation-generation.md`'s derived *external* API reference.
