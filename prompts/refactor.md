# Prompt: Refactor

**Use when:** improving the internal structure of existing code without changing its observable
behavior.
**Inputs required:** the file(s) or module to refactor, and the specific motivation (what's
actually wrong with the current structure — don't refactor without one).
**Loads context from:** `.ai/coding-philosophy.md`, `skills/clean-architecture.md`,
`skills/python.md`, existing tests for the code being touched.

```
You are refactoring {{target_files_or_module}}. Motivation: {{specific reason — e.g. "this
function mixes I/O and business logic, making it untestable without mocks"}}.

Ground rules:
1. Behavior does not change. Every existing test must still pass, unmodified, at the end — if a
   test needs to change, that's a signal this is not a pure refactor and should be re-scoped as a
   feature change with its own spec.
2. Before changing anything, confirm test coverage is sufficient to catch a behavior change in the
   area you're touching. If it isn't, write characterization tests first (see skills/testing.md) —
   this is the one case where tests are added before a refactor without a spec driving them,
   because they're protecting existing behavior, not specifying new behavior.
3. Apply /.ai/coding-philosophy.md directly:
   - Pull I/O out of domain/application code into adapters/ (§3, skills/clean-architecture.md).
   - Remove abstraction that isn't earning its keep — a refactor is a legitimate place to simplify,
     not just to add structure (§6, §9).
   - Fix names that no longer match /glossary/README.md terms exactly (§1).
4. Do not cross this refactor with unrelated changes — no drive-by formatting of untouched code, no
   bundling in a bug fix that isn't part of the stated motivation
   (/.ai/development-principles.md §7).
5. Do not change a published Engine contract as part of a refactor. If the refactor reveals that a
   contract itself is the problem, stop and write that up as its own spec/ADR instead
   (/.ai/development-principles.md §4).

Run `docker compose run --rm test` before and after to confirm the suite is green both times, and
`docker compose run --rm lint` at the end, per CLAUDE.md.

Summarize what changed structurally and why it's better, referencing the specific
coding-philosophy principle each change satisfies.
```

**Output:** behavior-preserving structural improvements, verified by an unchanged passing test
suite, with a clear rationale tied to the project's coding philosophy.
