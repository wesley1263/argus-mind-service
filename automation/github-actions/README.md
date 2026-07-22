# GitHub Actions

Specifications for every CI workflow, following `automation/workflow-spec-template.md`. All of
these are triggered by GitHub events (`pull_request`, `push`, `release`, `schedule`) — see
`automation/workflows/README.md` for the broader automation catalog that includes non-CI-triggered
work (bots, generated docs), and `automation/cicd/README.md` for environment/deployment strategy.

## Required Checks (block merge on every PR)

| Workflow | Enforces |
|---|---|
| [`lint.md`](lint.md) | `skills/python.md` toolchain (Ruff + mypy strict). |
| [`tests.md`](tests.md) | Full suite per `testing/README.md`. |
| [`coverage.md`](coverage.md) | Coverage floors (85% overall, 95% domain/application). |
| [`docker-build.md`](docker-build.md) | The image actually builds and boots. |
| [`architecture-validation.md`](architecture-validation.md) | Every rule in `testing/architecture-testing.md`. |
| [`specification-validation.md`](specification-validation.md) | Every changed spec is well-formed against `specs/template.md`. |
| [`spec-drift-check.md`](spec-drift-check.md) | Implemented specs match running code (PR-scoped part). |
| [`secret-scan.md`](secret-scan.md) | No credentials committed. |
| [`dependency-scan.md`](dependency-scan.md) | No newly-introduced Critical/High vulnerabilities. |
| [`documentation.md`](documentation.md) | Docs updated alongside the behavior they describe. |

## Advisory / Scheduled (report, don't block, unless noted)

| Workflow | Purpose |
|---|---|
| [`dead-code-detection.md`](dead-code-detection.md) | Flags unreachable code. |
| [`unused-dependency-detection.md`](unused-dependency-detection.md) | Flags declared-but-unused and undeclared-but-imported packages. |

## Release / Deploy Path

| Workflow | Trigger |
|---|---|
| [`versioning.md`](versioning.md) | Every merge to `main` — computes and validates the next version. |
| [`release.md`](release.md) | `main` push (evaluates) or manual dispatch — tags and publishes a release. |
| [`deploy.md`](deploy.md) | Per-environment, per `automation/cicd/environments.md`'s promotion rules. |

## Design Principle

Every workflow here runs the same commands a developer runs locally
(`docker compose run --rm lint` / `test`, per `CLAUDE.md` and `skills/docker.md`). None of these
specs invent a CI-only code path — if a check needs something not runnable locally, the Docker
Compose setup is incomplete and gets fixed there first.
