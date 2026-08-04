---
type: issue
state: closed
created: 2026-07-30T17:43:19Z
updated: 2026-07-30T22:05:29Z
author: vig-os-release-app[bot]
author_url: https://github.com/vig-os-release-app[bot]
url: https://github.com/vig-os/devkit-smoke-test/issues/326
comments: 1
labels: bug
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-31T05:34:43.902Z
---

# [Issue 326]: [Release 1.5.0 failed — automatic rollback](https://github.com/vig-os/devkit-smoke-test/issues/326)

Release 1.5.0 failed during the automated release workflow.

**Workflow Run:** [View logs](https://github.com/vig-os/devkit-smoke-test/actions/runs/30566768343)
**Release PR:** #325

**Automatic rollback attempted:**
- Release branch reset to pre-finalization state (best-effort)

**Tag status (forward-fix policy):**
- Release tags are not deleted by automation (workflow choice; GitHub immutable-release lock-in applies only after a release is **published** when that setting is enabled). If a tag was pushed before the failure, it remains on the remote.
- Use a new release candidate to validate fixes, then re-run the final release when ready.
- If a draft GitHub Release exists, manage it from the Releases UI; **publishing** locks the linked tag and assets when **immutable releases** are enabled.
---

# [Comment #1]() by [c-vigo]()

_Posted on July 30, 2026 at 10:05 PM_

The 1.5.0 publish failed on the release-immutability tombstone (tag name reserved after the published release was deleted during the devkit train restart). 1.5.0 was ghosted; the train shipped and promoted as 1.5.1. See vig-os/devkit#1318.

