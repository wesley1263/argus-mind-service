# Prompt: Generate Migration

**Use when:** a spec's Database section requires a schema change.
**Inputs required:** the spec's Database section, or a direct description of the schema change.
**Loads context from:** `skills/postgres.md`, the spec's Database section.

```
Generate a database migration for: {{schema_change_description_or_spec_path}}.

Before writing it:
1. Confirm which Engine's schema this belongs to (skills/postgres.md's schema-per-Engine
   convention: ingestion, knowledge, evidence, learning_state, generation, platform). A migration
   touches exactly one Engine's schema — if the requested change spans two, stop and split it into
   two migrations tied to two specs.
2. Confirm no foreign key crosses a schema boundary. Cross-Engine references are plain identifier
   columns with no REFERENCES constraint (skills/postgres.md).
3. If the spec declares the table append-only (e.g. an Evidence table per ADR-003), the migration
   must also revoke UPDATE/DELETE grants on it for the application's database role — this is not
   optional, it's the enforcement mechanism, not just the schema shape.
4. Name tables and columns exactly as the spec's Database section states, using
   /glossary/README.md terms for any column representing a business concept.
5. Decide additive vs. breaking (.ai/development-principles.md §4). A column drop, a type
   narrowing, or a new NOT NULL column without a default on an existing table is breaking — if so,
   confirm an ADR exists before generating the migration, and include the rollback/backfill
   approach in the migration's own description.

Generate the migration file(s) only — do not write the application code that uses the new schema in
the same step (that's prompts/implement-feature.md's job). Include the down/rollback migration.

Run the migration against a real containerized Postgres (`docker compose run --rm test` or the
project's migration-check command) to confirm it applies cleanly before treating this as done.
```

**Output:** a reviewed-ready migration (up and down), scoped to one Engine's schema, with grants
correctly restricted where the spec requires immutability, verified to apply cleanly.
