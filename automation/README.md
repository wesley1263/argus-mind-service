# Automation Layer

This folder specifies the repository's automation — CI, quality gates, AI-specific pipelines,
deployment strategy, and self-updating documentation. Per Phase 3's constraints, everything here is
written as a **markdown specification**, not as executable CI configuration
(`.github/workflows/*.yml`), migration scripts, or Dockerfiles. A GitHub Actions YAML file is
infrastructure-as-code — it is exactly the kind of "application code" this harness's every phase has
withheld until its own dedicated implementation phase. A specification here is precise enough that
implementing the real `.yml` is a mechanical translation, not a design exercise.

The two exceptions are `.github/PULL_REQUEST_TEMPLATE.md` and `.github/ISSUE_TEMPLATE/*.md`: a PR
or issue template's *only* form is markdown text — writing it is not "implementing" anything, it
*is* the finished artifact. Those live in `.github/` directly and are real, usable today.

## How to Read a Workflow Spec

Every workflow/pipeline document in this folder follows
[`workflow-spec-template.md`](workflow-spec-template.md): Trigger, Purpose, Steps, Gate, Inputs/
Secrets, Outputs, Related Docs. This is the same discipline `specs/template.md` established in
Phase 2, adapted to automation's shape instead of a feature's.

## Map of Every Requested Deliverable

| Phase 3 heading | Where it lives |
|---|---|
| GitHub Actions (Lint … Unused Dependency Detection) | [`github-actions/`](github-actions/README.md) — 15 workflow specs |
| Pull Request Template | [`/.github/PULL_REQUEST_TEMPLATE.md`](../.github/PULL_REQUEST_TEMPLATE.md) |
| Issue Templates | [`/.github/ISSUE_TEMPLATE/`](../.github/ISSUE_TEMPLATE) — 6 templates |
| Automation (Task Generation … Repository Health) | [`workflows/`](workflows/README.md) — 7 workflow specs (Spec/Architecture Validation cross-link to `github-actions/`, not duplicated) |
| Quality Gates | [`quality-gates.md`](quality-gates.md) |
| AI Automation | [`ai-automation/`](ai-automation/README.md) — 5 workflow specs |
| CI/CD (environments, Rollback, Blue-Green, Versioning, Tag Strategy) | [`cicd/`](cicd/README.md) |
| Documentation Automation | [`documentation-automation/`](documentation-automation/README.md) — 5 workflow specs |

## Governing Rules

These specs implement automation for a codebase that doesn't exist yet (Phase 1/2 are governance
and specs only — `docs/repository-structure.md`). Every spec here is written to be **buildable the
day `src/`, `tests/`, and `docker-compose.yml` exist**, and every gate it defines enforces a rule
already established in `.ai/`, `testing/`, or a spec in `/specs/` — automation here invents no new
policy, it only mechanizes what's already been decided. Where a spec would require a new policy
decision (e.g., a specific coverage threshold), that decision is stated explicitly and is a
candidate for `templates/decision.md` or an ADR if it turns out to be hard to reverse
(`.ai/development-principles.md` §2).

All CI/CD execution follows the absolute rule in `CLAUDE.md`: every job invokes the same
`docker compose run --rm <service>` commands a developer would run locally
(`skills/docker.md`, `skills/github-actions.md`) — CI is never a second, divergent definition of
"passing."
