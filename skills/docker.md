# Skill: Docker & Docker Compose

The absolute rule from `CLAUDE.md` applies here in full: **every command execution — build, lint,
test, run — happens through Docker Compose, never the local host.** This skill explains how that
setup is structured and why.

## Why the Host Is Never Used Directly

Running `pytest` or `ruff` on a developer's (or an agent's) local machine means the result depends
on whatever Python version, system libraries, and stray global packages happen to be installed
there. Docker Compose collapses that variable entirely: the container image is the single source of
truth for the runtime environment, identical for every contributor and identical to CI
(`skills/github-actions.md`). "Works on my machine" is not a category of bug this project accepts.

## Expected Services

`docker-compose.yml` (created alongside the first implementation work, per `CLAUDE.md`) defines, at
minimum:

| Service | Purpose |
|---|---|
| `app` | Runs the application (FastAPI server) for local development. |
| `db` | PostgreSQL, matching `skills/postgres.md`'s schema-per-Engine layout. |
| `lint` | Runs Ruff + mypy across the whole codebase. Invoked as `docker compose run --rm lint`. |
| `test` | Runs the full pytest suite (unit, integration, contract, architecture — `testing/`). Invoked as `docker compose run --rm test`. |

`lint` and `test` are one-shot (`run --rm`), not long-running — they execute, report, and exit,
exactly like a CI job would.

## Dockerfile Conventions

- **Multi-stage build**: a `builder` stage installs dependencies via Poetry; the final runtime stage
  copies only what's needed to run, keeping the shipped image free of build tooling and dev
  dependencies.
- Dependencies are installed from the committed `poetry.lock`, never resolved fresh at build time —
  a build is reproducible or it's wrong.
- The `lint`/`test` services can reuse the `builder` stage (they need dev dependencies like `ruff`,
  `mypy`, `pytest`) rather than the slim runtime stage.

## Local Development Loop

```bash
docker compose run --rm lint     # before every commit, per the Pre-commit workflow step
docker compose run --rm test     # full suite
docker compose up db             # just the database, if iterating with a local run of app
```

There is no supported "install Python locally and run things directly" path for this repository.
If a task seems to require it, that's a signal the Docker setup is missing something — fix the
Compose/Dockerfile setup, don't work around it.

## What Goes Wrong Without This Discipline

A lint or test pass that only ran on one contributor's host is not evidence the change is safe —
it's evidence about that host. Every `docker compose run --rm lint` / `test` invocation is
reproducible by anyone (or any agent) with Docker installed and nothing else, which is exactly what
makes the Definition of Done (`.ai/definition-of-done.md`) and Review Checklist
(`.ai/review-checklist.md`) checks trustworthy across many independent contributors and sessions.
