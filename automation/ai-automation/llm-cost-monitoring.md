# Workflow: LLM Cost Monitoring

- **Category:** AI Automation
- **Trigger:** `schedule` (daily aggregation), real-time budget-threshold alerting where the
  provider supports it
- **Blocking:** Advisory, except the hard budget cap below

## Purpose

Tracks spend on generative model calls (Generation Engine's content production, and any model-based
judge used by `prompt-evaluation-workflow.md`) against a budget, since Article I of
`.ai/constitution.md` treats the model as a swappable implementation detail — cost is exactly the
kind of operational factor that should be able to drive that swap decision, and can't unless it's
actually measured.

## Steps

1. Aggregate per-call cost data from Generation Engine's model-provider adapter
   (`skills/prompt-engineering.md`) — tagged by prompt version, content type, and (in aggregate,
   never per-Student in a way that could re-identify someone) target Learning Node, so cost can be
   attributed to *why* it was spent.
2. Aggregate separately: evaluation-run cost (`prompt-evaluation-workflow.md`,
   `golden-dataset-validation.md`) versus production-serving cost — evaluation cost is a fixed,
   predictable overhead per prompt change; production cost scales with Student activity and is the
   number that actually matters for unit economics.
3. Compare daily/weekly spend against the configured budget and against a per-request cost ceiling
   (catches a single runaway request — e.g. a retry loop — before it becomes a trend).
4. Feed the aggregated trend into `workflows/repository-health.md`'s weekly digest.

## Gate / Threshold

Advisory alerting at 80% of budget. **Hard block** (Generation Engine's model-calling adapter
refuses new requests until acknowledged) only if spend exceeds 100% of the configured hard cap — a
deliberate circuit breaker against a runaway cost incident, not a routine gate on normal usage.

## Inputs / Secrets

Read access to the model provider's billing/usage API.

## Outputs / Artifacts

Cost trend dashboard feed; budget-threshold alerts; per-prompt-version cost comparison (useful input
to `prompt-regression.md` — a "better" prompt that costs 10x more is a trade-off worth seeing
explicitly, not a pure win).

## Failure Behavior

Advisory alerts never block. The hard cap blocking production calls is treated as a Production
incident (paged, not just logged) — Students mid-Session losing Generation Engine availability is a
real-time reliability problem, not just a budget line item.

## Related Docs

`skills/prompt-engineering.md`, `automation/ai-automation/prompt-evaluation-workflow.md`,
`automation/workflows/repository-health.md`.
