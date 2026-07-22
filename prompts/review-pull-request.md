# Prompt: Review Pull Request

**Use when:** reviewing a complete pull request — process and CI state, not just the diff content
(see `prompts/review-code.md` for the code-only version this builds on).
**Inputs required:** the PR (link, number, or diff + description).
**Loads context from:** `.ai/review-checklist.md`, `.ai/definition-of-done.md`,
`docs/development-workflow.md`, `skills/github-actions.md`.

```
Review pull request {{pr_reference}} for merge-readiness.

1. Process check (docs/development-workflow.md): does the PR reference the spec it implements (and
   ADR, if applicable)? Is the branch named with the correct prefix
   (feat/test/refact/bugfix/hotfix/fix, per CLAUDE.md)? Is the change scoped to one logical unit of
   work, or does it bundle unrelated changes (.ai/development-principles.md §7)?

2. CI check (skills/github-actions.md): are the required checks (lint, test, and
   golden-dataset-eval where applicable) green? Do not treat a red or skipped check as something to
   reason around — it blocks merge, full stop.

3. Full code review: run through prompts/review-code.md's checklist (traceability, boundaries,
   ubiquitous language, evidence/learning-state integrity, validation gate, code quality, tests,
   documentation) against the actual diff.

4. Definition of Done: verify /.ai/definition-of-done.md and the spec's own Definition of Done
   section, item by item, against what's actually in the diff — not against the PR description's
   claims.

5. Changelog: confirm CHANGELOG.md was updated (Keep a Changelog format) as part of this PR, per
   CLAUDE.md's mandatory workflow, describing the change in terms a user/operator would understand.

Produce a verdict: Approve, or Changes Requested with a specific, actionable list — grouped by
which of the five checks above each item belongs to, so the author knows exactly what's blocking
and why. Do not approve with unresolved Constitutional issues (boundaries, language, integrity,
validation) deferred to a follow-up, per /.ai/review-checklist.md's closing note.
```

**Output:** a merge-readiness verdict covering process, CI, code, Definition of Done, and
changelog — not code review alone.
