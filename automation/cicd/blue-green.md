# CI/CD: Blue-Green Deployment

## Purpose

Production deploys (`environments.md`, `github-actions/deploy.md`) use a Blue-Green strategy so a
new version is fully live and verified before it receives any real Student traffic, and so reverting
is a traffic-routing change (`rollback.md`), never a rebuild-and-redeploy.

## How It Works Here

1. **Blue** is the currently-live version, serving all Production traffic.
2. **Green** is the new version being deployed: it starts up alongside Blue, fully isolated, with its
   own database migrations already applied (`skills/postgres.md` — migrations must be
   backward-compatible with Blue still running, per the Additive-by-Default rule in
   `.ai/development-principles.md` §4, since both versions run against the same database briefly).
3. Green runs `github-actions/deploy.md`'s post-deploy smoke test — including a real round-trip
   through `specs/study-session.md` — while receiving **zero** live traffic.
4. Traffic shifts from Blue to Green, either instantly or gradually (a canary percentage ramp, if
   the deployment target supports it) — the specific mechanism is an infrastructure choice made when
   this is implemented, not fixed by this spec.
5. Blue stays running, idle, for a configured bake period after the shift completes, so
   `rollback.md` can switch back to it instantly if a problem surfaces that the smoke test didn't
   catch.
6. Only after the bake period passes cleanly is Blue torn down.

## Why Migrations Must Be Backward-Compatible During the Overlap

Because Blue and Green run against the same schema simultaneously during the overlap window, a
migration that Green depends on must not break Blue's still-running queries. This means:

- Additive schema changes only during a Blue-Green window (new nullable columns, new tables) — never
  a rename or drop in the same deploy that introduces the code depending on the new shape.
- A breaking schema change ships as two deploys: one that adds the new shape while the old code
  still works unmodified, and a later one (after Blue is fully retired) that removes the old shape.

## Gate / Threshold

Green does not receive traffic until its smoke test passes. Full cutover does not complete until the
bake period elapses with no triggered alert.

## Related Docs

`automation/cicd/environments.md`, `automation/cicd/rollback.md`, `skills/postgres.md`,
`automation/github-actions/deploy.md`.
