---
type: issue
state: closed
created: 2026-07-08T17:13:23Z
updated: 2026-07-09T05:40:42Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/205
comments: 1
labels: none
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-07-18T04:48:06.486Z
---

# [Issue 205]: [fix: move pytest to dependency-group so just test survives groups-only sync (0.5.0)](https://github.com/vig-os/devkit-smoke-test/issues/205)

## Problem
The 0.5.0 devcontainer scaffold changed the managed `just sync` from `uv sync --all-extras --all-groups` to groups-only (`uv sync --all-groups`; extras are now opt-in). Our `pyproject.toml` declares `pytest`/`pytest-cov` as a `[project.optional-dependencies].dev` **extra**, so after a 0.5.0 deploy overwrites `justfile.project`, `just sync` no longer installs pytest and `just test` fails with `Failed to spawn: pytest`.

This broke the smoke-test for upstream `0.5.0-rc1` (deploy PR CI 'Tests' red → deploy PR never merges → upstream smoke-test dispatch fails; upstream vig-os/devcontainer#943).

## Fix
Move `pytest`/`pytest-cov` from the `[project.optional-dependencies].dev` extra to `[dependency-groups].dev` (PEP 735), so the default groups-only `just sync` installs them and `just test` works. Regenerate `uv.lock`. Keeps the Python smoke coverage (science/jupyter extras stay opt-in). PR to `dev`.

Refs: vig-os/devcontainer#943
---

# [Comment #1]() by [c-vigo]()

_Posted on July 9, 2026 at 05:40 AM_

Fixed by #206 (merged to `dev`). Moved `pytest`/`pytest-cov` to `[dependency-groups].dev` + regenerated `uv.lock`; verified `uv sync --all-groups` installs pytest and `uv run pytest` passes. Validated end-to-end by the 0.5.0-rc2 smoke-test (deploy + `just test` green): https://github.com/vig-os/devcontainer-smoke-test/actions/runs/28976856991. Closing.

