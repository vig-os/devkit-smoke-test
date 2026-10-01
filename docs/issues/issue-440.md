---
type: issue
state: open
created: 2026-10-01T12:16:22Z
updated: 2026-10-01T12:16:22Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/440
comments: 0
labels: none
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-10-01T12:24:40.723Z
---

# [Issue 440]: [Adopt DEVKIT_COMMIT_APP_ENVIRONMENT (commit-app environment binding)](https://github.com/vig-os/devkit-smoke-test/issues/440)

First live adoption of devkit#1710 (https://github.com/vig-os/devkit/issues/1710).

Binds the commit-App token-minting jobs to the `commit-app` deployment environment (policies: `main`/`dev`/`release/*`). Org secrets stay as fallback until a later train moves `COMMIT_APP_CLIENT_ID`/`COMMIT_APP_PRIVATE_KEY` to environment secrets.
