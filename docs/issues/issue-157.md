---
type: issue
state: closed
created: 2026-06-18T16:27:49Z
updated: 2026-06-18T16:47:03Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/157
comments: 0
labels: bug, area:ci
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:39:57.663Z
---

# [Issue 157]: [[BUG] Smoke-test prepare-release fails on empty Unreleased section](https://github.com/vig-os/devkit-smoke-test/issues/157)

## Description

The candidate release `0.3.6-rc1` dispatched the smoke-test, the deploy PR merged cleanly, but the downstream `prepare-release` step failed:

```
ERROR: CHANGELOG.md Unreleased section has no entries
```

This aborts the dispatch orchestration (the `Trigger and wait for prepare-release workflow` and `Dispatch summary` jobs fail, and upstream is notified of a smoke-test failure).

**Failed dispatch run:** https://github.com/vig-os/devcontainer-smoke-test/actions/runs/27758115273
**Failed prepare-release run:** https://github.com/vig-os/devcontainer-smoke-test/actions/runs/27758285399

## Steps to Reproduce

1. Publish a devcontainer release candidate (or `gh api repos/vig-os/devcontainer-smoke-test/dispatches` with `smoke-test-trigger`).
2. The deploy PR to `dev` merges; the listener then triggers this repo's `prepare-release`.
3. Observe `prepare-release` fail at "Verify CHANGELOG has Unreleased section entries".

## Expected Behavior

The smoke-test release pipeline runs end-to-end on every dispatch, regardless of whether the fixture has hand-authored changelog entries.

## Actual Behavior

`prepare-release` rejects the deploy because `## Unreleased` is empty.

## Root Cause

This repo is a **fixture**: nobody hand-authors changelog entries here. Each release freeze (`prepare-changelog prepare`) moves `## Unreleased` content into the dated section and resets `## Unreleased` to empty. On the next dispatch, the `prepare-release` gate requires `## Unreleased` to have at least one entry, so it can never pass on a no-op fixture deploy. (Upstream `devcontainer` passes the equivalent gate only because its Unreleased is populated with real changes.)

This is the consumer-side manifestation of the upstream issue vig-os/devcontainer#597.

## Fix

Seed a deploy entry into `## Unreleased` during the deploy step of `repository-dispatch.yml` when the section is empty (idempotent — existing entries are left untouched):

| Where | What | Tracked by |
|---|---|---|
| `assets/smoke-test/.github/workflows/repository-dispatch.yml` (upstream source) | Add a "Seed CHANGELOG Unreleased entry" step | vig-os/devcontainer#597 |
| `.github/workflows/repository-dispatch.yml` (this repo) | Apply the same seed step locally | **This issue** |

## Testing constraints

`repository_dispatch` events always run workflow files from the **default branch** (`main`). There is no way to test the dispatch listener from a topic branch or `dev`, so this fix must be merged all the way to `main` before the end-to-end dispatch path can be verified by re-triggering a smoke-test.

## Changelog Category

Fixed

