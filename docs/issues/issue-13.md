---
type: issue
state: closed
created: 2026-03-07T07:16:30Z
updated: 2026-03-09T15:46:52Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/13
comments: 0
labels: feature, area:ci
assignees: c-vigo
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:40:01.455Z
---

# [Issue 13]: [[FEATURE] Wire smoke-test dispatch listener to run RC CI variants](https://github.com/vig-os/devkit-smoke-test/issues/13)

### Description

Implement the smoke-test repo side of upstream cross-repo dispatch wiring so RC image publishes from `vig-os/devcontainer` can trigger smoke validation here.

Upstream context: https://github.com/vig-os/devcontainer/issues/173

### Problem Statement

This repository currently has the required building blocks but they are not yet wired for RC-driven smoke execution:

- `.github/workflows/repository-dispatch.yml` is a listener stub that only logs payload fields (from local issue #3)
- `.github/workflows/ci-container.yml` uses a hardcoded image tag (`ghcr.io/vig-os/devcontainer:latest`) (from local issue #6)

Without integration, a dispatched RC tag cannot be used to execute both CI variants against the released candidate image.

### Proposed Solution

Implement the smoke-test counterpart scope for upstream #173:

1. Upgrade `.github/workflows/repository-dispatch.yml` from stub to a functional dispatch handler:
   - Read `rc_tag` from `github.event.client_payload`
   - Validate `rc_tag` is non-empty
   - Trigger both CI variants (`ci.yml` bare-runner and `ci-container.yml` container)
   - Aggregate/report pass/fail status

2. Parameterize `.github/workflows/ci-container.yml` image tag:
   - Add `workflow_call` input for image tag (default `latest`)
   - Replace hardcoded container image tag with input-driven value
   - Preserve current PR and manual triggers

3. Optionally add `workflow_call` support to `.github/workflows/ci.yml` so dispatch handler can invoke both variants consistently.

### Alternatives Considered

- Keep dispatch listener as logging-only stub and trigger smoke checks manually: rejected; does not satisfy cross-repo wiring goal.
- Keep `ci-container.yml` hardcoded to `latest`: rejected; cannot validate specific RC candidates.

### Additional Context

- Upstream parent issue: https://github.com/vig-os/devcontainer/issues/173
- Local prior work:
  - #3 added the repository_dispatch listener stub
  - #6 added container CI workflow

### Impact

- Backward compatible; extends workflow wiring and parameterization only.
- Improves confidence that RC tags are validated in this smoke-test repository before final release promotion upstream.

### Changelog Category

Added

### Acceptance Criteria

- [ ] `repository-dispatch.yml` consumes dispatched `rc_tag` and executes smoke orchestration logic
- [ ] Both CI variants are run from dispatch flow (`ci.yml` and `ci-container.yml`)
- [ ] `ci-container.yml` supports dynamic image tag input (default remains `latest`)
- [ ] Existing PR/manual workflow behavior remains functional
- [ ] Upstream relationship to `vig-os/devcontainer#173` is documented in this issue
- [ ] TDD compliance (see `.cursor/rules/tdd.mdc`)
