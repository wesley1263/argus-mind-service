# Skill: Testing (Craft)

This skill is about how to write a *good individual test*, day to day. For the overall strategy —
which kinds of tests exist and what each is responsible for — see `testing/README.md` and the rest
of the `testing/` folder; this document is the practical companion, not a replacement.

## A Test Is Executable Specification

Per `.ai/coding-philosophy.md` §8, a test exists to pin down a real business rule, not to hit a
coverage number. Before writing a test, be able to state the rule it protects in one sentence — if
you can't, you're testing implementation detail, not behavior, and the test will break the next
time someone refactors without changing behavior.

## Structure: Arrange, Act, Assert

Every test follows the same three-part shape, with blank lines separating them so it's visible at a
glance:

```python
def test_confidence_decays_after_the_configured_window_with_no_new_evidence():
    # Arrange
    evidence = [evidence_at(days_ago=40, learning_node=NODE_A)]

    # Act
    state = compute_learning_state(evidence, as_of=now(), model=CONFIDENCE_MODEL_V1)

    # Assert
    assert state.confidence < INITIAL_CONFIDENCE_FROM(evidence)
```

Test names are full sentences describing the behavior, not `test_1` or `test_confidence_edge_case`.
A failing test's name should tell you what broke without opening the file.

## No Mocks in the Functional Core

Per `.ai/coding-philosophy.md` §3, `domain/` and most of `application/` are pure functions. Tests
for them construct plain input values and assert on plain output values — no mocking framework, no
patched imports. If you find yourself reaching for a mock to test something in `domain/`, that's a
signal I/O leaked into the core; fix the code, not the test.

## One Behavior per Test

Prefer several small, precisely-named tests over one large test asserting many unrelated things.
When a large test fails, a name like `test_generation_task_pipeline` tells you nothing about what
broke; five smaller, well-named tests each tell you exactly which rule failed.

## Fixtures Build Realistic, Minimal Data

Test fixtures construct the *smallest* valid instance of a domain object that satisfies a test's
needs — not a maximal "one big fixture reused everywhere" object that obscures which fields the
test actually cares about. A fixture builder (e.g. `evidence_at(days_ago=..., learning_node=...)`
with sensible defaults for everything else) keeps tests readable without repeating boilerplate.

## Adapters Get Integration Tests, Not Unit Tests With Mocks

Code in `adapters/` (a Postgres repository, an HTTP client) is tested against the real thing it
talks to — a real (containerized, per `skills/docker.md`) Postgres instance, not a mocked driver.
Mocking the database in an adapter test only proves the mock behaves as configured; it doesn't
prove the adapter works. See `testing/integration-testing.md`.

## A Red Test Is Not Optional

Per the mandatory workflow in `CLAUDE.md`, a test is written and confirmed failing (Red) before the
implementation that makes it pass (Green) is written. A test that was never seen failing is not
trustworthy — it may be passing for the wrong reason (a typo in the assertion, a fixture that never
exercises the code path).
