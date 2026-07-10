---
type: issue
state: closed
created: 2026-02-25T08:34:09Z
updated: 2026-03-03T07:02:09Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/3
comments: 0
labels: feature
assignees: c-vigo
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:40:02.442Z
---

# [Issue 3]: [[FEATURE] Add repository_dispatch listener stub for smoke-test orchestration](https://github.com/vig-os/devkit-smoke-test/issues/3)

### Description

Add a minimal `repository_dispatch` listener workflow to `vig-os/devcontainer-smoke-test` as a stub for cross-repo smoke-test orchestration from `vig-os/devcontainer`.

### Problem Statement

The upstream bootstrap scope expects this repo to include a dispatch listener stub so cross-repo wiring can be completed in follow-up work. Without this listener, integration sequencing for smoke-test orchestration remains blocked/ambiguous.

### Proposed Solution

- Add a new workflow under `.github/workflows/` triggered by `repository_dispatch`
- Keep implementation intentionally minimal (log payload fields and exit successfully)
- Do not add gating logic or RC tag mutation yet (deferred to follow-up integration work)

### Alternatives Considered

- Implement full cross-repo dispatch and release gating now — rejected as out of scope for this step.
- Keep listener out of repo until full integration — rejected because it delays dependency sequencing.

### Additional Context

- Related upstream tracking: [vig-os/devcontainer#170](https://github.com/vig-os/devcontainer/issues/170)
- Follow-up integration work is expected to build on this stub.

### Impact

- Backward compatible; adds workflow infrastructure only.
- Improves readiness for cross-repo smoke-test automation.

### Changelog Category

Added

### Acceptance Criteria

- [ ] `repository_dispatch` listener workflow exists in `.github/workflows/`
- [ ] Workflow runs successfully when manually triggered via dispatch API
- [ ] Listener is documented as a stub for follow-up integration
- [ ] TDD compliance (see `.cursor/rules/tdd.mdc`)
