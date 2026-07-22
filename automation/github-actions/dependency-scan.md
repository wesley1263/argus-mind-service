# Workflow: Dependency Scan

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (when `pyproject.toml`/`poetry.lock` changes), `schedule` (daily, full
  dependency tree regardless of PR activity)
- **Blocking:** Required check on PR (new/changed dependencies); advisory on schedule (files an
  issue instead of blocking anything)

## Purpose

Catches known-vulnerable dependencies before they're introduced, and flags newly-disclosed
vulnerabilities in dependencies already in `poetry.lock`.

## Steps

1. Resolve the dependency tree from `poetry.lock` (never re-resolve against `pyproject.toml` alone
   — the lock file is the source of truth, per `skills/docker.md`'s reproducibility principle).
2. Run a vulnerability scanner (e.g. `pip-audit` or `osv-scanner`) against the resolved tree.
3. On PR: fail if the change introduces a dependency with a known **Critical** or **High**
   severity vulnerability with no available fixed version.
4. On schedule: if a previously-clean dependency now has a disclosed vulnerability, open (or update)
   a tracking issue using `templates/bug.md`'s shape, tagged `dependency-vulnerability`.

## Gate / Threshold

PR blocks on any **Critical** or **High** severity finding introduced by the diff. **Medium/Low**
findings are reported but do not block, to avoid the gate becoming noise that gets routinely
overridden.

## Inputs / Secrets

None required for the scan itself; issue-creation requires standard repo-write permissions (not a
separate secret).

## Outputs / Artifacts

Scan report artifact; PR status check; scheduled-run tracking issues for newly-disclosed
vulnerabilities in existing dependencies.

## Failure Behavior

PR: blocks merge until the vulnerable dependency is removed, upgraded, or (rarely, with explicit
written justification in the PR) formally accepted via a `templates/decision.md` entry linked in
the PR description — silent override is not possible.

## Related Docs

`skills/python.md`, `templates/bug.md`, `templates/decision.md`.
