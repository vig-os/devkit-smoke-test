---
type: issue
state: open
created: 2026-06-22T07:32:40Z
updated: 2026-06-22T07:32:40Z
author: vig-os-release-app[bot]
author_url: https://github.com/vig-os-release-app[bot]
url: https://github.com/vig-os/devcontainer-smoke-test/issues/168
comments: 0
labels: bug
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-06-22T09:48:30.308Z
---

# [Issue 168]: [Release 0.3.7 failed — automatic rollback](https://github.com/vig-os/devcontainer-smoke-test/issues/168)

Release 0.3.7 failed during the automated release workflow.

**Workflow Run:** [View logs](https://github.com/vig-os/devcontainer-smoke-test/actions/runs/27936796080)
**Release PR:** #

**Automatic rollback attempted:**
- Release branch reset to pre-finalization state (best-effort)

**Tag status (forward-fix policy):**
- Release tags are not deleted by automation (workflow choice; GitHub immutable-release lock-in applies only after a release is **published** when that setting is enabled). If a tag was pushed before the failure, it remains on the remote.
- Use a new release candidate to validate fixes, then re-run the final release when ready.
- If a draft GitHub Release exists, manage it from the Releases UI; **publishing** locks the linked tag and assets when **immutable releases** are enabled.
