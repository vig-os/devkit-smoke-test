---
type: issue
state: closed
created: 2026-02-24T12:48:29Z
updated: 2026-03-03T07:02:09Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/1
comments: 0
labels: chore
assignees: c-vigo
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:40:02.927Z
---

# [Issue 1]: [[CHORE] Minimal smoke-test repo changes to make CI/CD pass](https://github.com/vig-os/devkit-smoke-test/issues/1)

### Chore Type
CI / Build change

### Description
Apply the smallest possible set of changes inside `vig-os/devcontainer-smoke-test` so the repository CI/CD passes reliably in GitHub Actions.

This issue is the smoke-test-repo sibling of [vig-os/devcontainer#169](https://github.com/vig-os/devcontainer/issues/169), focused only on fixes in this repository (no template/source changes in `vig-os/devcontainer` unless separately tracked).

### Acceptance Criteria
- [x] Identify current CI/CD failing jobs and root causes in this repo
- [x] Apply minimal, targeted changes in this repo only
- [x] CI workflow(s) pass on a PR to `dev`
- [ ] CI workflow(s) pass after merge to `main`
- [x] Document any temporary workaround and open follow-up issue(s) if broader upstream fixes are needed

### Implementation Notes
- Prefer config/workflow adjustments over structural refactors
- Keep diffs minimal and traceable
- If a failure depends on upstream template/action behavior, isolate with the least invasive repo-local mitigation and link upstream issue

### Related Issues
Related to [vig-os/devcontainer#169](https://github.com/vig-os/devcontainer/issues/169)

### Priority
Medium

### Changelog Category
No changelog needed

### Additional Context
Goal: unblock smoke validation quickly by making only essential CI/CD fixes in the smoke-test repository.
