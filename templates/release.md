# Release Template

> Use this shape for each version entry in `CHANGELOG.md`, following
> [Keep a Changelog](https://keepachangelog.com/) — the format `CLAUDE.md`'s mandatory workflow
> requires updating at the end of every change. Entries describe user/operator-visible impact, not
> implementation detail — "what changed for someone using or operating the platform," not "which
> files changed."

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- New capability, stated from the Student/operator's perspective, with a link to its spec.

### Changed
- Behavior that changed for existing users, and why.

### Deprecated
- Anything scheduled for removal, and the recommended alternative.

### Removed
- Anything actually removed in this release.

### Fixed
- Bug fixes, referencing the bug if tracked via `templates/bug.md`.

### Security
- Security-relevant fixes — described with enough care to be useful without disclosing an
  exploitable detail before users have had a chance to update.
```

## Release Checklist

- [ ] Every entry traces to a merged spec, ADR, or bug fix — no undocumented changes.
- [ ] Version number follows semantic versioning: breaking contract changes
      (`.ai/development-principles.md` §4) bump the major version.
- [ ] `docker compose run --rm test` is green on the exact commit being released.
- [ ] Migration notes included if this release requires an operator to run new migrations
      (`skills/postgres.md`).
