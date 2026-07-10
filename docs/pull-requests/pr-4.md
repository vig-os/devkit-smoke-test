---
type: pull_request
state: closed (merged)
branch: feature/3-repository-dispatch-listener-stub → dev
created: 2026-02-25T08:58:14Z
updated: 2026-02-25T09:00:03Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/pull/4
comments: 0
labels: none
assignees: none
milestone: none
projects: none
merged: 2026-02-25T08:59:49Z
synced: 2026-07-10T15:45:59.510Z
---

# [PR 4](https://github.com/vig-os/devkit-smoke-test/pull/4) ci: add repository_dispatch listener stub

## Description

Adds a stub GitHub Actions workflow that listens for `repository_dispatch` events and logs payload metadata for smoke-test integration groundwork. Also updates Unreleased changelog entries to reflect both the dispatch listener and prior bootstrap/setup alignment work.

## Type of Change

- [ ] `feat` -- New feature
- [ ] `fix` -- Bug fix
- [ ] `docs` -- Documentation only
- [ ] `chore` -- Maintenance task (deps, config, etc.)
- [ ] `refactor` -- Code restructuring (no behavior change)
- [ ] `test` -- Adding or updating tests
- [x] `ci` -- CI/CD pipeline changes
- [ ] `build` -- Build system or dependency changes
- [ ] `revert` -- Reverts a previous commit
- [ ] `style` -- Code style (formatting, whitespace)

### Modifiers

- [ ] Breaking change (`!`) -- This change breaks backward compatibility

## Changes Made

- `.github/workflows/repository-dispatch.yml`
  - Adds a dedicated `repository_dispatch` workflow for `smoke-test-trigger`
  - Introduces a single stub job that logs key dispatch metadata from `github.event` payload fields
  - Uses constrained permissions (`contents: read`) and a short timeout
- `CHANGELOG.md`
  - Adds an Unreleased entry for issue `#3` describing the repository dispatch listener stub
  - Adds an Unreleased entry for issue `#1` documenting bootstrap/setup and lock-state alignment

## Changelog Entry

### Added

- **Bootstrap scripts and lock state aligned for CI validation** ([#1](https://github.com/vig-os/devcontainer-smoke-test/issues/1))
  - Updated repository bootstrap/setup behavior to match the current development and CI flow
  - Refreshed pre-commit and dependency lock state to keep validation runs consistent
- **repository_dispatch listener stub** ([#3](https://github.com/vig-os/devcontainer-smoke-test/issues/3))
  - Added a dedicated workflow that listens for `repository_dispatch` events
  - Logs dispatch payload metadata as a minimal foundation for later cross-repo integration

## Testing

- [x] Tests pass locally (`just test`)
- [ ] Manual testing performed (describe below)

### Manual Testing Details

Automated tests passed via `just test-pytest`.

## Checklist

- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have updated the documentation accordingly (edit `docs/templates/`, then run `just docs`)
- [x] I have updated `CHANGELOG.md` in the `[Unreleased]` section (and pasted the entry above)
- [ ] My changes generate no new warnings or errors
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Any dependent changes have been merged and published

## Additional Notes

N/A

Refs: #3



---
---

## Commits

### Commit 1: [1a9e1d5](https://github.com/vig-os/devkit-smoke-test/commit/1a9e1d52a9a2b925b8b849f7e53f0b12bbd2aeb9) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:36 AM
feat(ci): add repository_dispatch listener stub, 28 files modified (.github/workflows/repository-dispatch.yml)

### Commit 2: [2dfe086](https://github.com/vig-os/devkit-smoke-test/commit/2dfe0864c3b8250c3efd7d4c9adca6b086518315) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:41 AM
docs(changelog): add unreleased entry for dispatch listener, 3 files modified (CHANGELOG.md)

### Commit 3: [11c1bb2](https://github.com/vig-os/devkit-smoke-test/commit/11c1bb2809dc38d3a913f166f56377ce955dd4cd) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:49 AM
docs(changelog): add entry for bootstrap scripts, 4 files modified (CHANGELOG.md)
