# Regression Testing

## Principle

A bug, once fixed, does not come back silently. This isn't a separate test suite so much as a
discipline applied across every other layer in `testing/`: every fix ships with proof that the
specific failure it addresses is now caught automatically.

## Every Bug Fix Starts With a Failing Test

Per `templates/bug.md` and the mandatory Red/Green workflow in `CLAUDE.md`, a bug fix begins by
writing a test that reproduces the reported failure and confirming it fails against the current
code — *before* touching the fix itself. That test:

- Lives in whichever layer actually caught (or should have caught) the bug: `unit-testing.md` if
  it's a domain rule that was wrong, `integration-testing.md` if an adapter misbehaved,
  `architecture-testing.md` if a boundary was violated and nothing caught it structurally.
- Stays in the suite permanently after the fix — it is the regression test. There is no separate
  "regression suite" to maintain; regression coverage is just the accumulated set of every bug
  fix's Red test, still running on every PR.

## Golden Dataset and Prompt Evaluation Regressions

For the score-based layers (`golden-dataset.md`, `prompt-evaluation.md`), "regression" means a
scenario's score dropping below its previously-accepted baseline, not just a new failure appearing.
A real-world extraction or generation failure that reveals a gap in the dataset is added as a new
scenario (`golden-dataset.md`'s "Growing the Dataset" section) — the equivalent, at this layer, of
writing a failing test first.

## Full-Suite Discipline

Every PR runs the complete suite (`docker compose run --rm test`), not just tests related to the
files it touches. A change in one Engine can regress another Engine's expectations of a published
contract (`integration-testing.md`'s contract tests) in ways that are invisible if only "relevant"
tests are selected. Regression protection depends on this being non-negotiable — see
`skills/github-actions.md`'s required-checks stance.

## When a Regression Reveals a Missing Layer

If a bug reaches production and no existing test category would plausibly have caught it — not
unit, not integration, not architecture, not golden dataset — that's a signal the testing strategy
itself has a gap, not just that one test was missing. Treat that as its own follow-up: either an
existing `testing/*.md` document needs a new rule added (like a new row in
`architecture-testing.md`'s enforcement table), or the gap genuinely reveals a new category of risk
worth naming explicitly.
