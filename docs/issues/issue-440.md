---
type: issue
state: open
created: 2026-10-01T12:16:22Z
updated: 2026-10-01T12:26:38Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/440
comments: 1
labels: none
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-10-02T08:00:58.400Z
---

# [Issue 440]: [Adopt DEVKIT_COMMIT_APP_ENVIRONMENT (commit-app environment binding)](https://github.com/vig-os/devkit-smoke-test/issues/440)

First live adoption of devkit#1710 (https://github.com/vig-os/devkit/issues/1710).

Binds the commit-App token-minting jobs to the `commit-app` deployment environment (policies: `main`/`dev`/`release/*`). Org secrets stay as fallback until a later train moves `COMMIT_APP_CLIENT_ID`/`COMMIT_APP_PRIVATE_KEY` to environment secrets.
---

# [Comment #1]() by [c-vigo]()

_Posted on October 1, 2026 at 12:26 PM_

## Pre-train verification (2026-10-01) — passed

Merged to `dev` in #441. Environment `commit-app` now holds `COMMIT_APP_CLIENT_ID` / `COMMIT_APP_PRIVATE_KEY` as environment secrets (names verified via the API). The org secrets remain as a fallback for now.

| Check | Run | Result |
|---|---|---|
| `sync-issues` dispatched on `dev` (bound `sync` job) | [36861638896](https://github.com/vig-os/devkit-smoke-test/actions/runs/36861638896) | success — `commit-app` deployment created, token minted, `commit-action-bot` pushed the mirror commit to `dev` |
| `sync-issues` dispatched from an out-of-policy branch | [36861495393](https://github.com/vig-os/devkit-smoke-test/actions/runs/36861495393) | refused as intended: *Branch "feature/440-env-negative-probe" is not allowed to deploy to commit-app due to environment protection rules.* (probe branch deleted) |

Note: an earlier positive run (36861471469) shows `cancelled` only because the negative probe was queued in the same `sync-issues` concurrency group; all its steps had succeeded.

## Remaining

- [ ] Next devkit train's candidate exercises `release-core.yml` `finalize` (reusable callee, `required: false` + job-level environment) live — cannot be dry-run tested.
- [ ] Train after next: drop this repo from the org `COMMIT_APP_*` secrets' selected repositories — blocked on vig-os/devkit#1793 (listener `deploy` job not yet bound).

Keeping this issue open until the `finalize` leg is proven.


