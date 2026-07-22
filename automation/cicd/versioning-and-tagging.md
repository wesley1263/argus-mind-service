# CI/CD: Versioning and Tag Strategy

## Scheme: Semantic Versioning

`MAJOR.MINOR.PATCH`, computed automatically by `github-actions/versioning.md`:

- **MAJOR** — any breaking change to a published Engine contract (`.ai/development-principles.md`
  §4): a field removed, a type narrowed, a required parameter added to an existing contract. Always
  paired with an ADR per that same section.
- **MINOR** — a new capability: a new endpoint, a new additive contract field, a new Generation Task
  content type. Anything traceable to a new `specs/*.md` Goal.
- **PATCH** — a bug fix or internal-only change with no contract impact (a refactor, a dependency
  patch update, a prompt tuning that doesn't change its declared version's external behavior
  contract — though per `skills/prompt-engineering.md` most prompt changes do warrant their own
  prompt version, tracked separately from the application's release version).

## How the Bump Is Determined

`github-actions/versioning.md` classifies each merged PR by its branch prefix and labels
(`CLAUDE.md`'s `feat/`, `fix/`, `hotfix/`, `bugfix/`, `refact/`, `test/`) and cross-checks any PR
touching a published contract against the actual contract diff — a PR can't claim "non-breaking" if
the automated diff says otherwise (see that workflow's Steps for the enforcement detail). The release
version is the highest bump implied by any PR since the last release (one MAJOR-labeled PR in the
batch makes the whole release MAJOR, even if every other PR was a PATCH).

## Git Tags

- Format: `vMAJOR.MINOR.PATCH` (e.g. `v1.4.2`), applied to the exact commit `github-actions/release.md`
  released, immutable once pushed — a tag is never moved or force-updated to point elsewhere.
- Container images (`github-actions/docker-build.md`) are tagged identically, so `v1.4.2` the git tag
  and `v1.4.2` the image tag always refer to the same built artifact — this is what makes
  "promote, never rebuild" (`environments.md`) verifiable rather than just asserted.

## Pre-Release / Environment Tags

- **Development** deploys (every `main` merge) are tagged with the commit SHA only — they are not
  semantically versioned, since not every merge is a release candidate.
- **Staging** carries the same `vMAJOR.MINOR.PATCH` tag as the Release it was built from — nothing
  new is tagged for Staging specifically, reinforcing that Staging validates the *exact* artifact
  Production will receive.

## Contract Versioning (Separate From Release Versioning)

An individual published Engine contract (`.ai/architecture.md` §4) may carry its own version
independent of the overall service's release version — a MINOR service release can introduce Contract
v2 for one Engine while every other Engine's contracts stay at v1. `github-actions/versioning.md`'s
contract-diff step tracks these independently so a consumer can pin to a specific contract version,
not just a service release, per `.ai/development-principles.md` §4's additive-by-default rule.

## Related Docs

`automation/github-actions/versioning.md`, `automation/github-actions/release.md`,
`automation/github-actions/docker-build.md`, `.ai/development-principles.md` §4.
