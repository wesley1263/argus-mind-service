# Workflow: Unused Dependency Detection

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (when `pyproject.toml` changes), `schedule` (monthly, full audit)
- **Blocking:** Advisory

## Purpose

Keeps `pyproject.toml`'s dependency list honest — every declared dependency is actually imported
somewhere, and every import has a corresponding declared dependency (the latter catches a
transitive dependency being relied on directly by accident, which breaks the moment the direct
dependency that pulled it in changes).

## Steps

1. Statically scan all `import`/`from ... import` statements across `src/` and `tests/`.
2. Compare against `pyproject.toml`'s declared dependencies (production and dev groups separately —
   a production-only import must not depend on a dev-only-declared package).
3. Flag: declared-but-unused packages; imported-but-undeclared packages (relying on a transitive
   dependency directly).
4. On the monthly scheduled run, additionally check for available minor/patch updates on unused
   packages before recommending removal (no point recommending removing something about to be
   needed by an already-planned upgrade — cross-check against `workflows/dependency-updates.md`'s
   open PRs).

## Gate / Threshold

Advisory only for the same reason as `dead-code-detection.md`: a dependency imported only in a code
path not exercised by static analysis (e.g., an optional adapter selected by runtime configuration)
can look unused without being unused.

## Inputs / Secrets

None.

## Outputs / Artifacts

PR comment on `pyproject.toml` changes; monthly summary issue.

## Failure Behavior

Never blocks. A repeated finding across several monthly runs (the same package flagged 3+ times) is
escalated to a proper `templates/task.md` entry rather than left as a recurring, ignorable comment.

## Related Docs

`skills/python.md`, `automation/workflows/dependency-updates.md`.
