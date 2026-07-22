---
type: issue
state: open
created: 2026-07-21T16:28:26Z
updated: 2026-07-21T16:28:26Z
author: vig-os-release-app[bot]
author_url: https://github.com/vig-os-release-app[bot]
url: https://github.com/vig-os/devkit-smoke-test/issues/289
comments: 0
labels: bug
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-22T05:18:34.465Z
---

# [Issue 289]: [Release 1.4.1 failed — automatic rollback](https://github.com/vig-os/devkit-smoke-test/issues/289)

Release 1.4.1 failed during the automated release workflow.

**Workflow Run:** [View logs](https://github.com/vig-os/devkit-smoke-test/actions/runs/29848630377)
**Release PR:** #288

**Automatic rollback attempted:**
- Release branch reset to pre-finalization state (best-effort)

**Tag status (forward-fix policy):**
- Release tags are not deleted by automation (workflow choice; GitHub immutable-release lock-in applies only after a release is **published** when that setting is enabled). If a tag was pushed before the failure, it remains on the remote.
- Use a new release candidate to validate fixes, then re-run the final release when ready.
- If a draft GitHub Release exists, manage it from the Releases UI; **publishing** locks the linked tag and assets when **immutable releases** are enabled.
