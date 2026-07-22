# Workflow: Docker Build

- **Category:** GitHub Actions
- **Trigger:** `pull_request` (all branches), `push:main`, `release` (tag)
- **Blocking:** Required check on PR/main; production-gating on release

## Purpose

Proves the multi-stage Dockerfile described in `skills/docker.md` actually builds a runnable
image, and that the image that will eventually be deployed (`cicd/environments.md`) is built from
exactly the code under review — not assembled by hand later.

## Steps

1. Build the `builder` stage and the runtime stage of the Dockerfile (`skills/docker.md`).
2. Run a smoke test against the built runtime image: start it, confirm the process boots and a
   basic health endpoint responds, then stop it. This is not the full test suite (that's
   `tests.md`) — it only proves the image itself is runnable.
3. On `pull_request`/`push:main`: build only, tagged with the commit SHA, pushed to a CI-internal
   cache (not a public registry).
4. On `release`: build, tag per `cicd/versioning-and-tagging.md`'s scheme, and push to the real
   container registry that `deploy.md` pulls from.

## Gate / Threshold

Image builds successfully and passes the boot smoke test. Image size and build time are tracked
(not gated initially) as inputs to `workflows/code-metrics.md`.

## Inputs / Secrets

Container registry credentials (release builds only), scoped to push access on the specific
repository/image name.

## Outputs / Artifacts

Built image, tagged and cached (PR/main) or pushed to the registry (release); build-time and
image-size metrics.

## Failure Behavior

Blocks merge (PR/main) or blocks the release from proceeding to `deploy.md` (release trigger). A
failed boot smoke test is treated as a Required-check failure, not a warning.

## Related Docs

`skills/docker.md`, `automation/cicd/versioning-and-tagging.md`, `automation/github-actions/deploy.md`.
