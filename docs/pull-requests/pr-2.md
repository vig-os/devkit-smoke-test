---
type: pull_request
state: closed (merged)
branch: bugfix/1-make-ci-cd-pass → dev
created: 2026-02-24T15:57:14Z
updated: 2026-02-25T08:26:58Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/pull/2
comments: 0
labels: none
assignees: c-vigo
milestone: none
projects: none
merged: 2026-02-25T08:26:50Z
synced: 2026-07-10T15:46:00.930Z
---

# [PR 2](https://github.com/vig-os/devkit-smoke-test/pull/2) chore: align setup scripts and lockfile for CI validation

## Description

This PR prepares the smoke-test repository for reliable CI workflow validation by aligning project setup scripts, dependency lock state, and supporting docs/changelog updates.

## Type of Change

- [ ] `feat` -- New feature
- [ ] `fix` -- Bug fix
- [ ] `docs` -- Documentation only
- [x] `chore` -- Maintenance task (deps, config, etc.)
- [ ] `refactor` -- Code restructuring (no behavior change)
- [ ] `test` -- Adding or updating tests
- [ ] `ci` -- CI/CD pipeline changes
- [ ] `build` -- Build system or dependency changes
- [ ] `revert` -- Reverts a previous commit
- [ ] `style` -- Code style (formatting, whitespace)

### Modifiers

- [ ] Breaking change (`!`) -- This change breaks backward compatibility

## Changes Made

- Updated `.devcontainer/scripts/setup-gh-repo.sh`
  - Adjusted repository setup behavior used during environment/bootstrap flow.
- Updated `.pre-commit-config.yaml`
  - Simplified or removed config entries no longer needed by the current workflow.
- Updated `uv.lock`
  - Refreshed locked dependency state to match current project configuration.
- Updated `README.md`
  - Documented project purpose/scope and current setup expectations.
- Updated `CHANGELOG.md`
  - Added Unreleased entries covering scaffold/docs/devcontainer image updates.

## Changelog Entry

### Added

- Deployed initial project scaffold
- Created [README.md](./README.md)
- Updated devcontainer image to development version [5753eb2](https://github.com/vig-os/devcontainer/commit/5753eb2aafda99268a199ea22de344345cacacff) ([#1](https://github.com/vig-os/devcontainer-smoke-test/issues/1))

## Testing

- [x] Tests pass locally (`just test`)
- [ ] Manual testing performed (describe below)

### Manual Testing Details

N/A

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

Purpose of this PR is to validate CI workflow behavior from a realistic maintenance branch touching setup, lockfile, and docs/changelog updates.

Refs: #1



---
---

## Commits

### Commit 1: [52d196e](https://github.com/vig-os/devkit-smoke-test/commit/52d196e106b7795a9779489f9723fd6b4aebec19) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:10 PM
docs: create README with project purpose and scope for devcontainer smoke test, 40 files modified (README.md)

### Commit 2: [c080dd5](https://github.com/vig-os/devkit-smoke-test/commit/c080dd53a8ce5bb143694d5800d39da1e00603cb) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:18 PM
chore: update uv.lock, 108 files modified (uv.lock)

### Commit 3: [003a545](https://github.com/vig-os/devkit-smoke-test/commit/003a545e4d10135c93883b33d66d0997e248ebcf) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:48 PM
chore: update devcontainer image and template, 41 files modified (.devcontainer/scripts/setup-gh-repo.sh, .pre-commit-config.yaml)

### Commit 4: [a10e73c](https://github.com/vig-os/devkit-smoke-test/commit/a10e73cf0cd6ddb03b22df5231bd35030fb8e284) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:52 PM
docs: update CHANGELOG, 4 files modified (CHANGELOG.md)

### Commit 5: [bdb2f5c](https://github.com/vig-os/devkit-smoke-test/commit/bdb2f5c1c41bb520344c520acad69ddf75f209cf) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 04:11 PM
feat: install pre-commit in setup action, 9 files modified (.github/actions/setup-env/action.yml)

### Commit 6: [3d37608](https://github.com/vig-os/devkit-smoke-test/commit/3d37608dff583fb2a27872d6eb2b6e35113f807b) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 07:27 AM
chore: remove pre-commit hooks that are unavalable in CI, 53 files modified (.pre-commit-config.yaml)

### Commit 7: [bc75ec6](https://github.com/vig-os/devkit-smoke-test/commit/bc75ec602e139f9c535357217826009c90667f74) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:12 AM
chore: update devcontainer image and template, 15 files modified (.devcontainer/docker-compose.yml, .devcontainer/justfile.base, uv.lock)
