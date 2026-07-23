# Prompt: Generate Tests

**Use when:** a spec's Acceptance Criteria need to become failing tests (the Red step), or existing
code needs coverage added.
**Loads:** `memory/coding-standards.md`'s Testing section, the spec's Acceptance Criteria.

```
Generate tests for {{target}}.
{{If a spec exists: "Base these on the Acceptance Criteria in " + spec_path + " — one test (or a
small focused group) per AC, named after the behavior, not the AC number."}}

Follow memory/coding-standards.md:
- Arrange/Act/Assert, full-sentence test names
  (test_confidence_decays_after_the_configured_window_with_no_new_evidence, not test_decay_1).
- No mocks for domain/application code — plain input values, plain output assertions. If a test
  needs a mock, I/O has leaked into the core; fix the code, not the test.
- Adapters get integration tests against the real thing in a container — never a mocked driver.
- One behavior per test.

Classify each test: unit, integration, or architecture. A published Engine contract gets a contract
test that runs against both producer and consumer, not a one-sided unit test.

Confirm each test actually fails (new behavior) or passes (backfilled coverage) before treating it
as done. List any Acceptance Criterion you couldn't translate into a test and why — that's usually
a sign the AC itself needs clarifying, not something to skip silently.
```

**Output:** named, categorized tests mapped to Acceptance Criteria, confirmed to actually execute.
