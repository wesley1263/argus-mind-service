# CI/CD: Environments

## The Four Environments

| Environment | Purpose | Deploys from | Trigger |
|---|---|---|---|
| **Local** | A contributor's (or agent's) own iteration loop. | Uncommitted working tree. | `docker compose up` (`skills/docker.md`) — never a CI trigger. |
| **Development** | Continuous integration environment reflecting the true state of `main`. | `main`, on every merge. | Automatic on `push:main` (`github-actions/deploy.md`). |
| **Staging** | Pre-production validation against a tagged, releasable version. | A GitHub Release (`github-actions/release.md`). | Automatic on release publish. |
| **Production** | What Students actually use. | The same artifact validated in Staging — never rebuilt. | Manual `workflow_dispatch` with an approval gate. |

## The One Rule That Matters: Promote, Never Rebuild

The exact container image (`github-actions/docker-build.md`) that passed Staging is the same image
promoted to Production — identified by its immutable digest, not rebuilt from source a second time.
Rebuilding at each stage would reintroduce the "works on my machine" risk `skills/docker.md` exists
to eliminate, one environment later.

## Configuration Differences Between Environments

Only externalized configuration differs between environments (database connection strings, model
provider credentials, feature-relevant limits) — never application behavior gated by an
environment-name check in code. If Generation Engine needs to behave differently in Staging (e.g.,
a cheaper model to control `ai-automation/llm-cost-monitoring.md` spend during testing), that's a
configuration value injected at deploy time (`skills/docker.md`, `skills/github-actions.md`
§Secrets), not an `if environment == "staging"` branch in `domain/` or `application/` code — that
would violate the functional-core purity `.ai/coding-philosophy.md` §3 requires.

## Database Per Environment

Each environment has its own fully isolated PostgreSQL instance (`skills/postgres.md`'s
schema-per-Engine layout applies identically within each). Production Evidence
(`specs/evidence-engine.md`) never flows into Staging or Development for testing purposes — realistic
test data is synthetic or a deliberately anonymized subset, never a live copy of Student data, per
the same spirit as `.ai/constitution.md`'s data-integrity stance.

## Promotion Gate: Development → Staging → Production

- **Development → Staging** happens automatically at release time (`github-actions/release.md`)
  once every Required check has passed.
- **Staging → Production** requires: the post-deploy smoke test in `github-actions/deploy.md`
  passing in Staging, plus explicit manual approval — Production is the one environment this harness
  never auto-promotes into without a human (or an explicitly designated approving agent role)
  confirming it.

## Related Docs

`automation/github-actions/deploy.md`, `automation/github-actions/docker-build.md`,
`automation/github-actions/release.md`, `automation/cicd/blue-green.md`,
`automation/cicd/rollback.md`.
