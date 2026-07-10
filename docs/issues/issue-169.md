---
type: issue
state: closed
created: 2026-06-22T09:34:14Z
updated: 2026-06-22T11:33:16Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/169
comments: 1
labels: bug
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:39:56.624Z
---

# [Issue 169]: [[BUG] Smoke-test dispatch not idempotent across candidate→final on same base version](https://github.com/vig-os/devkit-smoke-test/issues/169)

## Description

When a base version is released through **both** a candidate pass (`X.Y.Z-rcN`) and a
final pass (`X.Y.Z`), the smoke-test dispatch orchestration is **not idempotent** across
the shared `release/X.Y.Z` branch and the per-base-version CHANGELOG entry. The final
pass dates the `## [X.Y.Z]` heading **before** `release-core` validation runs, which then
fails because it requires the `## [X.Y.Z] - TBD` placeholder.

## Observed (0.3.7)

Upstream `devcontainer` ran `publish-candidate 0.3.7` then `finalize-release 0.3.7`
against the same base version. The final smoke-test dispatch failed:

- Failed run: https://github.com/vig-os/devcontainer-smoke-test/actions/runs/27936796080
- Job "Release Core / Validate Release Core" → `ERROR: CHANGELOG.md does not contain '## [0.3.7] - TBD'`

CHANGELOG history on the reused `release/0.3.7` branch:

| Commit | Effect on `[0.3.7]` heading |
|--------|------------------------------|
| `8ff042f7` freeze (RC dispatch) | created `## [0.3.7] - TBD` |
| `84bd64e5` deploy (final dispatch) | dated it → `## [0.3.7](…) - 2026-06-22` |
| validate (`release-core.yml:180`) | requires `- TBD` → exit 1 |

A second failure mode also appears: the `deploy` idempotency check
(`grep -q "Smoke-test deploy of <tag>"`, `repository-dispatch.yml`) matches the bullet
left inside the stale `[X.Y.Z]` section, so the deploy step skips re-seeding `Unreleased`,
and `prepare-release` then aborts with an empty Unreleased section (the same class as
the already-fixed #157).

## Root cause

`repository-dispatch.yml` re-runs the whole `deploy → prepare-release → release → promote`
orchestration on **every** dispatch against a **reused** `release/X.Y.Z` branch and a
per-base-version CHANGELOG entry that a prior candidate pass already froze.
`prepare-changelog prepare` stacks a new heading without deduping, and
`prepare-changelog finalize` requires a `- TBD` entry and is not idempotent
(`packages/vig-utils/src/vig_utils/prepare_changelog.py:386-390`).

## Proposed fix (directions)

- Reset the target version entry to `## [X.Y.Z] - TBD` (or recreate `release/X.Y.Z`
  from a clean `dev`) at the start of each dispatch, so candidate→final on one base
  version is idempotent; and/or
- Make `prepare-changelog finalize` a no-op when the entry is already dated for the same
  version; and/or
- Scope the `deploy` idempotency check to the `Unreleased` section only, not the whole file.
- Note: `repository-dispatch.yml` is a synced template (header note) — fix at the template
  source in the `devcontainer` repo and re-sync.

## Workaround used to recover 0.3.7

Cleaned `dev` CHANGELOG (removed the polluted `[0.3.7]` sections), deleted the stale
`release/0.3.7` branch, and re-dispatched the final smoke-test.

---

# [Comment #1]() by [c-vigo]()

_Posted on June 22, 2026 at 09:52 AM_

Root-cause fix tracked upstream where the `repository-dispatch.yml` template and `prepare-changelog` tooling live: vig-os/devcontainer#612. This downstream issue tracks the symptom.

