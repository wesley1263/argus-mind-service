# Workflow: Automatic Task Generation

- **Category:** Automation
- **Trigger:** A spec's Status changes to `Approved` (detected via PR merge that changes that
  field), or `workflow_dispatch` against a specific spec path
- **Blocking:** N/A (produces planning artifacts, doesn't gate anything)

## Purpose

Mechanizes `prompts/generate-tasks.md`: the moment a spec is Approved, produce its ordered task
breakdown automatically rather than waiting for someone to run the prompt by hand, so implementation
can start immediately with a plan already in place.

## Steps

1. Detect the spec that transitioned to `Approved` (diff the `Status:` field in the merged PR).
2. Run an AI agent invocation of `prompts/generate-tasks.md` against that spec, with the same
   context-loading the prompt specifies (`.ai/architecture.md` §5, `templates/task.md`).
3. Open a tracking issue (or a checklist comment on the originating spec PR) listing the generated
   tasks in `templates/task.md`'s shape, ordered by dependency.
4. Tag each generated task with its owning Engine and layer (Migration / Domain / Application /
   Port / Adapter / Contract Test / Integration) for filtering.

## Gate / Threshold

N/A — advisory output, not a gate. A human (or the next agent session) still decides whether the
generated breakdown is correct before starting implementation.

## Inputs / Secrets

Access to invoke the AI agent runtime used for `prompts/*` (model API credentials,
`skills/github-actions.md` §Secrets).

## Outputs / Artifacts

A tracking issue or checklist with the ordered task list, ready to drive
`prompts/implement-feature.md` one task at a time.

## Failure Behavior

If task generation fails or produces an incomplete breakdown, it's flagged for manual follow-up —
this workflow never silently leaves a spec Approved with no plan and no signal that planning
failed.

## Related Docs

`prompts/generate-tasks.md`, `templates/task.md`, `specs/README.md`.
