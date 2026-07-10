---
type: issue
state: closed
created: 2026-03-09T14:21:41Z
updated: 2026-03-09T15:36:16Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/22
comments: 0
labels: bug, area:ci
assignees: c-vigo
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:40:00.328Z
---

# [Issue 22]: [[BUG] Repository dispatch fails: dependency-review job requests pull-requests: write beyond caller ceiling](https://github.com/vig-os/devkit-smoke-test/issues/22)

## Description

Repository dispatch triggers fail with a startup error because the `ci.yml` reusable workflow declares a job-level permission (`pull-requests: write`) that exceeds both its own top-level permissions and the caller's permission ceiling.

**Failed run:** https://github.com/vig-os/devcontainer-smoke-test/actions/runs/22857407072

**Error:**
> Error calling workflow 'vig-os/devcontainer-smoke-test/.github/workflows/ci.yml@68ee1fe'. The nested job 'dependency-review' is requesting 'pull-requests: write', but is only allowed 'pull-requests: none'.

## Steps to Reproduce

1. Trigger a repository dispatch:
   ```bash
   gh api repos/vig-os/devcontainer-smoke-test/dispatches \
     -X POST \
     -f event_type='smoke-test-trigger' \
     -f client_payload[event_type]='rc-published' \
     -f client_payload[rc_tag]='latest' \
     -f client_payload[source_repo]='manual-test'
   ```
2. Observe startup failure in the Actions run.

## Expected Behavior

The dispatch workflow should start successfully. The `dependency-review` job (which only runs on `pull_request` events) should be skipped without causing a parse-time permissions error.

## Actual Behavior

GitHub validates permissions at parse time before evaluating `if` conditions. Since `dependency-review` requests `pull-requests: write` but the effective ceiling is `none`, the entire workflow is rejected.

## Root Cause

Two compounding issues:

1. **`ci.yml` internal inconsistency (upstream):** Top-level `permissions` declares only `contents: read`, making `pull-requests` implicitly `none`. The `dependency-review` job then requests `pull-requests: write` at the job level, which exceeds the top-level declaration. This is fine for direct triggers (`pull_request`, `workflow_dispatch`) but invalid in a `workflow_call` context.

2. **`repository-dispatch.yml` caller ceiling:** Top-level `permissions` only grants `contents: read`. Even after fixing `ci.yml`, the caller must also grant `pull-requests: write` so the called workflow's effective permissions include it.

## Fix

Two changes required:

| Where | What | Tracked by |
|---|---|---|
| `ci.yml` (upstream template) | Add `pull-requests: write` to top-level `permissions` | vig-os/devcontainer#173 |
| `repository-dispatch.yml` (this repo) | Add `pull-requests: write` to top-level `permissions` | **This issue** |

After vig-os/devcontainer#173 is merged and the `ci.yml` template is synced, apply the local fix to `repository-dispatch.yml`:

```yaml
permissions:
  contents: read
  pull-requests: write
```

## Testing constraints

`repository_dispatch` events always run workflow files from the **default branch** (`main`). Unlike `pull_request` or `workflow_dispatch` triggers, there is no way to test dispatch workflows from a topic branch or `dev`.

This means both fixes must be merged all the way to `main` before we can verify the dispatch path:

1. Merge the upstream `ci.yml` template fix (vig-os/devcontainer#173) and sync it to this repo's `main`.
2. Merge the `repository-dispatch.yml` permissions fix to `main`.
3. Re-trigger the dispatch to verify.

The `ci.yml` changes can be independently validated via a PR (which exercises the `pull_request` trigger), but the end-to-end dispatch path requires `main`.

## Environment

- **Workflow:** `.github/workflows/repository-dispatch.yml`
- **Called workflow:** `.github/workflows/ci.yml`
- **Commit:** 68ee1fe

## Changelog Category

Fixed
