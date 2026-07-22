# Skill: PostgreSQL

How this project's relational storage is structured. One PostgreSQL instance serves the whole
modular monolith (ADR-001), but its schema still enforces the Engine boundaries the code enforces —
the database is not an escape hatch around `.ai/architecture.md` §4.

## Schema-per-Engine

Each Engine owns exactly one PostgreSQL schema, matching its package name:
`ingestion`, `knowledge`, `evidence`, `learning_state`, `generation`, `platform`. A table lives in
the schema of the Engine that owns the concept it represents — see the **Database** section of each
spec in `/specs/` for the current, authoritative list of tables and their owning schema.

## No Cross-Schema Foreign Keys

A table in one Engine's schema never has a foreign key into another Engine's schema. Cross-Engine
references (e.g., an `evidence_records` row referencing a `learning_node_id` that Knowledge Engine
owns) are stored as plain identifier columns with **no `REFERENCES` constraint** across the schema
boundary. This is deliberate, not an oversight: a cross-schema foreign key is a hidden coupling
that bypasses the published-contract rule in `.ai/architecture.md` §4 — the database would silently
enforce a dependency that code review is supposed to catch. Within a single Engine's own schema,
foreign keys are used normally.

## Enforcing Immutability at the Database Level

Where a spec declares a table append-only (Evidence Engine's `evidence_records`, per
`specs/evidence-engine.md`), that guarantee is enforced by the database, not just by application
code discipline: the application's database role has no `UPDATE` or `DELETE` grant on that table.
"We just won't call `UPDATE` in the code" is not sufficient — see ADR-003 and the corresponding Risk
entry in `specs/evidence-engine.md`.

## Migrations

- Every schema change ships as a migration, generated and reviewed like any other code change —
  never applied by hand against a running database.
- A migration is scoped to one Engine's schema per change, mirroring how specs and PRs are scoped
  (`.ai/development-principles.md` §7). A migration that touches two Engines' schemas in one file
  is a signal the change should be two migrations landing with two specs.
- Migrations run through Docker Compose (`skills/docker.md`), identically in local dev and CI
  (`skills/github-actions.md`) — never against a developer's local Postgres outside the container.
- A migration that removes a column or narrows a type is a breaking change and follows
  `.ai/development-principles.md` §4 (additive by default; breaking changes need an ADR if they
  affect a published contract that reads from that column).

## Naming

Tables and columns use `snake_case` and match `/glossary/README.md` terms exactly where a column
represents a business concept (`confidence`, `learning_node_id`, `evidence_id`) — see
`.ai/constitution.md` Article VI. Internal-only concepts stay internal in the schema too: Ingestion
Engine's `chunks` table is a real table, but nothing outside the `ingestion` schema — and nothing
outside Ingestion Engine's own code — ever queries it directly (ADR-002).

## Query Discipline

- Domain and application code never construct raw SQL inline in `application/` — SQL/ORM queries
  live in `adapters/`, behind a repository-shaped port, exactly like any other I/O
  (`skills/clean-architecture.md`).
- Learning State's `projections` table (see `specs/learning-state-engine.md`) is explicitly a
  cache over the Evidence log — it must always be safe to `TRUNCATE` and fully rebuild from
  `evidence.evidence_records`. Treat any schema change to it as a change to a rebuildable cache, not
  to a source of truth.
