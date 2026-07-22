# Prompt: Generate Tests

**Use when:** a spec's Acceptance Criteria need to become actual failing tests (the Red step), or
existing code needs test coverage added.
**Inputs required:** the spec (if any) and the target code/module.
**Loads context from:** `skills/testing.md`, `testing/README.md`, the spec's Acceptance Criteria.

```
Generate tests for {{target}}.
{{If a spec exists: "Base these directly on the Acceptance Criteria in " + spec_path + " — one
test (or a small focused group) per AC, named after the AC's behavior, not its number."}}

Follow skills/testing.md:
- Arrange/Act/Assert structure, with the test name stating the behavior in full-sentence form
  (test_confidence_decays_after_the_configured_window_with_no_new_evidence, not test_decay_1).
- No mocks for domain/application code — construct plain input values and assert on plain output
  values. If testing this requires a mock, flag that the code under test likely has I/O leaking
  into its core (.ai/coding-philosophy.md §3) rather than reaching for a mocking framework anyway.
- Adapters (Postgres repositories, HTTP clients, external model clients) get integration tests
  against the real thing, in a container (testing/integration-testing.md) — never a mocked driver.
- One behavior per test. Prefer several small, precisely-named tests over one large test.

Classify each test you generate by the testing/ category it belongs to (testing/README.md):
unit, integration, contract, or architecture. If you're testing a published Engine contract,
generate it as a contract test that can run against both producer and consumer, not as a one-sided
unit test (testing/README.md, .ai/development-principles.md §6).

Confirm each generated test actually fails against the current code (if the code doesn't exist yet)
or actually passes against the current code (if you're backfilling coverage) before treating it as
done — do not generate tests you haven't verified execute correctly.

List, explicitly, any Acceptance Criterion from the spec you could NOT translate into a test and
why — this usually means the AC itself is underspecified and the spec needs a follow-up, not that
the test should be skipped silently.
```

**Output:** a set of named, categorized tests mapped directly to Acceptance Criteria (or to
identified coverage gaps), confirmed to actually execute and fail/pass as expected.
