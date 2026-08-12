---
type: issue
state: open
created: 2026-08-07T20:14:40Z
updated: 2026-08-07T20:14:40Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/341
comments: 0
labels: none
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-08-08T03:23:50.949Z
---

# [Issue 341]: [Adopt human-approval gate in dispatch listener (vig-os/devkit#1391)](https://github.com/vig-os/devkit-smoke-test/issues/341)

The org now forbids workflow-token PR approvals (vig-os/org-config#122), which broke the dispatch listener's auto-approve step during the devkit 1.7.0-rc3 chain (vig-os/devkit#1391).

Mirror the fixed `repository-dispatch.yml` from the devkit asset ahead of the rc4 dispatch: the listener executes from dev at trigger time, so the deploy PR alone cannot fix the in-flight run. Candidate dispatches leave the release PR unapproved; the final dispatch polls for a human approval.

Content must be identical to the rendered devkit asset on `release/1.7.0` so the next deploy PR carries no listener diff.
