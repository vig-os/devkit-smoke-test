---
type: issue
state: closed
created: 2026-02-25T11:06:43Z
updated: 2026-03-03T20:00:16Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/6
comments: 0
labels: feature
assignees: c-vigo
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:40:01.981Z
---

# [Issue 6]: [[FEATURE] Add container-based CI workflow for smoke-test repo](https://github.com/vig-os/devkit-smoke-test/issues/6)

### Description

Add a `ci-container.yml` workflow to `vig-os/devcontainer-smoke-test` that runs CI jobs inside the devcontainer image using GitHub Actions `container:`.

This repository is the implementation target first; once validated, the pattern can be migrated into the template/source repo.

### Problem Statement

The devcontainer image includes CI tooling, but we do not yet validate that an end-to-end CI workflow succeeds when executed inside that container in GitHub-hosted Actions.

Current checks validate tool presence and lifecycle separately, but not the full "run CI inside `container:`" use case.

### Proposed Solution

- Create `.github/workflows/ci-container.yml`
- Run lint/test jobs with `container: ghcr.io/vig-os/devcontainer:latest`
- Mirror the existing CI steps used by bare-runner CI where possible
- Capture and document behavior differences vs bare-runner execution (checkout behavior, user context, no DinD assumptions)

### Alternatives Considered

- Keep validating only with bare-runner workflows: does not cover containerized CI runtime behavior.
- Delay until template-repo migration: increases risk by deferring smoke validation.

### Additional Context

- Adapted from upstream tracking: https://github.com/vig-os/devcontainer/issues/171
- Related local sequencing:
  - Depends on bootstrap readiness from #1
  - Should coexist with existing CI and not replace it
- Follow-up work may migrate the proven workflow pattern into the template/source repo.

### Impact

- Backward compatible: adds a new workflow alongside existing CI
- Improves confidence that the devcontainer image is CI-ready in real Actions `container:` jobs

### Changelog Category

Added

### Acceptance Criteria

- [ ] `.github/workflows/ci-container.yml` exists in this repo
- [ ] CI jobs run inside `ghcr.io/vig-os/devcontainer:latest` via `container:`
- [ ] Lint and test jobs pass inside the container workflow
- [ ] Both bare-runner CI and container CI run on PRs
- [ ] Quirks/differences vs bare-runner CI are documented in-repo
- [ ] TDD compliance (see `.cursor/rules/tdd.mdc`)
