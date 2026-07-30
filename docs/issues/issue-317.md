---
type: issue
state: open
created: 2026-07-30T14:33:52Z
updated: 2026-07-30T14:33:52Z
author: vig-os-release-app[bot]
author_url: https://github.com/vig-os-release-app[bot]
url: https://github.com/vig-os/devkit-smoke-test/issues/317
comments: 0
labels: bug
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-30T21:45:03.128Z
---

# [Issue 317]: [Release 1.5.0 failed — automatic rollback](https://github.com/vig-os/devkit-smoke-test/issues/317)

Release 1.5.0 failed during the automated release workflow.

**Workflow Run:** [View logs](https://github.com/vig-os/devkit-smoke-test/actions/runs/30551816884)
**Release PR:** #315

**Automatic rollback attempted:**
- Release branch reset to pre-finalization state (best-effort)

**Tag status (forward-fix policy):**
- Release tags are not deleted by automation (workflow choice; GitHub immutable-release lock-in applies only after a release is **published** when that setting is enabled). If a tag was pushed before the failure, it remains on the remote.
- Use a new release candidate to validate fixes, then re-run the final release when ready.
- If a draft GitHub Release exists, manage it from the Releases UI; **publishing** locks the linked tag and assets when **immutable releases** are enabled.
