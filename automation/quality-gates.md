# Quality Gates

The specific, numeric or rule-based thresholds that block a merge. Where a workflow in
`github-actions/` or `workflows/` *measures* something, this document is the single place that
states the *threshold* — so a threshold never has to be hunted for across a dozen workflow specs,
and never quietly drifts between them.

## Coverage

- Overall: **≥85%**. Domain/Application layers (`skills/clean-architecture.md`): **≥95%** — they're
  pure functions, cheap to test fully (`testing/unit-testing.md`).
- Patch coverage (changed lines in a PR): **≥80%**, even if overall coverage is healthy.
- Enforced by: `github-actions/coverage.md`.

## Complexity (Cognitive)

- No function may exceed a cognitive complexity score of **15** (deeply nested conditionals, mixed
  control flow) — a function past this point is nearly always better expressed as several smaller,
  named functions per `.ai/coding-philosophy.md` §9 ("small, explicit, boring").
- Enforced by: a static analysis step inside `github-actions/lint.md` (Ruff's complexity rules, or
  an equivalent linter plugin), reported per-function so the specific offending function is
  identified, not just a file-level average.

## Cyclomatic Complexity

- No function may exceed a cyclomatic complexity of **10**.
- `domain/` functions specifically are held to **6** — per `.ai/coding-philosophy.md` §3, the
  functional core should be simple enough that its branches map directly to distinct business rules
  a reviewer can enumerate by reading the function once.
- Enforced by: the same static analysis step as Complexity, reported separately since cyclomatic and
  cognitive complexity can diverge (a function can have many branches but still read simply, or few
  branches but be cognitively tangled).

## Architecture Rules

- The full rule set is `testing/architecture-testing.md`'s enforcement table (Engine boundaries,
  Shared Kernel dependency direction, Chunk containment, no direct Confidence writes, Evidence
  immutability grants, the Validation gate, framework-import layering).
- Gate: **100% pass**, no tiering — these encode Constitutional Articles
  (`.ai/constitution.md` III–V).
- Enforced by: `github-actions/architecture-validation.md`.

## Naming

- Every business-concept identifier (class, function, variable, table/column, event name) matches
  `/glossary/README.md` exactly (`.ai/constitution.md` Article VI).
- Checked by: a naming-lint step that extracts glossary terms as a canonical wordlist and flags
  near-miss identifiers (e.g. `LearningNod`, `confidance`) as likely-unintentional deviations for
  human confirmation — this check is **advisory** (naming judgment has real false positives, e.g. a
  local loop variable abbreviating a long glossary term) but is surfaced prominently in
  `github-actions/lint.md`'s output rather than silently skipped.

## Folder Structure

- Every Engine package under `src/argus/` follows the exact `domain/ → application/ → ports/ →
  adapters/` shape from `.ai/architecture.md` §5 — no additional top-level subfolder inside an
  Engine package without an ADR justifying the deviation (`.ai/development-principles.md` §2).
- Enforced by: a structural check (does the expected subfolder set exist, are there no
  unrecognized top-level siblings) as part of `github-actions/architecture-validation.md`.

## DDD Validation

- Every entity has an explicit identity field; every value object is immutable and has no identity
  field (`skills/ddd.md`).
- No aggregate boundary is crossed by a single transactional write spanning two Engines' schemas
  (`skills/postgres.md`'s no-cross-schema-foreign-key rule is the storage-level proxy for this).
- Enforced by: a combination of the naming/type-shape check above (frozen/immutable dataclasses for
  declared Value Objects) and `architecture-validation.md`'s schema-boundary check.

## Clean Architecture Validation

- No `domain/` or `application/` module imports a framework (FastAPI, the DB driver, an AI vendor
  SDK) or another Engine's package (`skills/clean-architecture.md`).
- `application/` never accepts a framework-specific type (a FastAPI `Request`, a SQLAlchemy
  `Session`) as a parameter (`skills/clean-architecture.md`'s "layer violation to watch for").
- Enforced by: import-graph analysis, part of `github-actions/architecture-validation.md`'s
  framework-import-layering rule.

## Changing a Threshold

Every number on this page is a policy decision, not a law of nature. Changing one is a
`templates/decision.md` entry if it's a routine recalibration (e.g., raising coverage from 85% to
90% once the codebase matures), or an ADR if lowering a Constitutional-rule-adjacent gate (Coverage
floor is a candidate for a `decision.md`; loosening Architecture Rules' 100%-pass stance is not —
that would need a Constitution amendment per `.ai/constitution.md` Article IX).
