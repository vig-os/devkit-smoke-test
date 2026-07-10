---
type: issue
state: closed
created: 2026-07-09T16:13:52Z
updated: 2026-07-10T10:11:00Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/221
comments: 0
labels: chore, dependencies, area:ci
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-10T15:39:54.512Z
---

# [Issue 221]: [Complete Dependabot→Renovate migration (remove dependabot.yml, cover pep621)](https://github.com/vig-os/devkit-smoke-test/issues/221)

## Problem

This repo is in a half-migrated state between Dependabot and Renovate:

- `renovate.json` enables **only** `github-actions` (`"enabledManagers": ["github-actions"]`).
- `.github/dependabot.yml` is **still active**, managing `github-actions` + `pip` + `npm`.

Consequences:

- **GitHub Actions are double-managed** by both Dependabot and Renovate.
- **Python (pip) is Dependabot-only** — Renovate does not cover it here.
- The **npm** ecosystem in `dependabot.yml` is dead (no `package.json`).

This produced a backlog of stale Dependabot PRs (#136–#140 pip, #152 actions), all now closed as obsolete: `pyproject.toml` has since moved from `>=` ranges to exact `==` pins at versions that already meet/exceed the proposed bumps, and actions are owned by Renovate.

## Target state (matches the devcontainer template SSoT)

The upstream template `assets/workspace/renovate.json` in `vig-os/devcontainer` ships Renovate-only with `"enabledManagers": ["github-actions", "pep621", "npm"]` and **no `dependabot.yml`**. Bring this repo in line:

- [ ] Remove `.github/dependabot.yml`.
- [ ] Broaden `renovate.json` `enabledManagers` to `["github-actions", "pep621", "npm"]` (matching the template; `pep621` is the fix that makes Renovate cover `pyproject.toml`).

## Notes

- Internal tooling change, no user-visible impact → no CHANGELOG entry.
- After merge, Renovate owns actions + Python; Dependabot stops opening PRs.
