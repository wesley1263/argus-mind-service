# Workflow Spec Template

> Copy this to start a new automation spec anywhere under `automation/`. It describes a workflow
> precisely enough to implement as real CI configuration later, without writing that configuration
> now (see `automation/README.md` for why).

# Workflow: <Name>

- **Category:** GitHub Actions | Automation | AI Automation | CI/CD | Documentation Automation
- **Trigger:** <on: pull_request / push:main / schedule(cron) / workflow_dispatch / release,
  stated precisely — a workflow with a vague trigger can't be reviewed for whether it runs too
  often or too rarely>
- **Blocking:** Required check (blocks merge) | Advisory (reports, does not block) | Scheduled
  (not PR-gated)

## Purpose

*What this workflow exists to catch or produce, in one or two sentences, and which rule/spec it
enforces or automates (link it).*

## Steps

*High-level, numbered, tool-agnostic where possible but naming a concrete real-world tool where one
is assumed — this is what makes the spec implementable rather than aspirational. Not YAML; prose or
pseudocode.*

1. …

## Gate / Threshold

*The specific pass/fail (or score) condition, stated as a number or rule, not "reasonable
quality." If this spec is establishing a new numeric threshold rather than citing one that's
already fixed elsewhere, say so explicitly — a new threshold is a policy decision worth a
`templates/decision.md` entry.*

## Inputs / Secrets

*Configuration and credentials this workflow needs, and where they come from
(`skills/github-actions.md` §Secrets) — never hardcoded.*

## Outputs / Artifacts

*What this workflow produces: a status check, a posted PR comment, an uploaded artifact, a
generated file committed back to the repo, a deployed environment.*

## Failure Behavior

*What happens when this workflow fails or its gate isn't met — blocked merge, alert, automatic
rollback, retry. "Nothing in particular happens" is not an acceptable answer for a Required check.*

## Related Docs

*Every spec, skill, or `.ai/` document this workflow enforces or depends on.*
