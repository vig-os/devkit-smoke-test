---
type: issue
state: closed
created: 2026-03-08T20:08:04Z
updated: 2026-03-09T06:57:43Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/16
comments: 1
labels: bug, area:ci
assignees: c-vigo
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:40:00.995Z
---

# [Issue 16]: [[BUG] Changelog overstates Dependabot action version bumps](https://github.com/vig-os/devkit-smoke-test/issues/16)

## Description

Copilot review on PR #15 (dev → main) identified that `CHANGELOG.md` claims Dependabot bumped `actions/cache` to v5, `actions/checkout` to v6, and `actions/upload-artifact` to v7, but every workflow in the repo still pins these to v4 SHAs:

- `actions/checkout@34e114...` = v4
- `actions/upload-artifact@ea165f...` = v4
- `actions/cache@0057852...` = v4

The changelog entry must accurately reflect what PR #15 ships.

## Steps to Reproduce

1. Read `CHANGELOG.md` lines 27-29 under `### Changed`
2. Compare against actual action SHAs in `.github/workflows/ci-container.yml`, `ci.yml`, `sync-issues.yml`
3. Observe the versions are still v4, contradicting the changelog

## Expected Behavior

Changelog entry accurately describes what was shipped — that Dependabot branches were reconciled without changing the effective major action versions.

## Actual Behavior

Changelog claims `actions/cache-5.0.3`, `actions/checkout-6.0.2`, `actions/upload-artifact-7.0.0` were merged, implying those versions are now in use.

## Environment

- Source: Copilot review on PR #15

## Possible Solution

Reword the entry:

```markdown
- **Reconciled Dependabot GitHub Actions branches** ([#6](...))
  - Merged Dependabot branches for `actions/cache`, `actions/checkout`, and `actions/upload-artifact`
  - Workflow pins remain at current major versions; branch merges captured minor config updates
```

## Changelog Category

Fixed

Refs: #15
---

# [Comment #1]() by [c-vigo]()

_Posted on March 9, 2026 at 06:57 AM_

Closing — Dependabot PRs #17–#20 have since landed on `dev`, updating all workflow action pins to match the changelog claims:

- `actions/checkout` → v6.0.2 (`de0fac2e...`)
- `actions/upload-artifact` → v7.0.0 (`bbbca2dd...`)
- `actions/cache` → v5.0.3 (`cdf6c1fa...`)

The changelog entry is now accurate. No fix needed.

Agent-blocklist script defects (wrong `project_root`, missing `vig_utils`, inverted `IN_CONTAINER` guard) were filed upstream as vig-os/devcontainer#238.

