---
type: issue
state: open
created: 2026-09-29T11:07:20Z
updated: 2026-09-29T13:21:50Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/issues/438
comments: 1
labels: chore, dependencies, python:uv
assignees: none
milestone: none
projects: none
parent: none
children: none
synced: 2026-09-30T07:59:59.510Z
---

# [Issue 438]: [chore(deps): drop leftover jupyter/science extras from the smoke payload](https://github.com/vig-os/devkit-smoke-test/issues/438)

### Chore Type

Dependency update

### Description

`pyproject.toml` still carries the `dev` (jupyter, ipykernel), `science` (numpy, scipy, pandas, matplotlib) and `all` optional extras, plus a `[tool.uv] constraint-dependencies` block pinning jupyter-stack floors. They are left over from the devkit Python project template (vig-os/devkit `f1b19c34`), which devkit removed on 2026-07-08 (vig-os/devkit `a026454e`, language-neutral scaffold). This repo kept its copy.

Nothing uses them. CI installs only the dependency groups (`just sync` = `uv sync --all-groups`, extras opt-in), which resolve to 14 packages (hatchling, pytest, pytest-cov, rich and their deps). `uv.lock` still locks all 122 packages, because uv locks every extra.

**All 14 open Dependabot alerts come from the unused extras:** anyio (critical GHSA-82r6-8w77-94w6 and 2 more), tornado, mistune, jupyterlab and soupsieve. Checked with `uv export --frozen --all-groups`: the set CI actually installs contains none of those packages. Dependabot security updates were enabled here by vig-os/org-config#304 and opened #433-#437. Those PRs cannot merge: they target `main` directly, which skips `dev` and the release train, and they fail the managed branch-name gate (vig-os/devkit#1755).

The smoke payload exists to exercise the scaffold's Python path (lint, test, the `DEVKIT_LANGUAGES=python` marker gate) on a real repo. That needs a minimal project, not a Jupyter/science stack.

### Acceptance Criteria

- [ ] `pyproject.toml` drops the `dev`, `science` and `all` extras and the `[tool.uv] constraint-dependencies` block. `dependencies = []` and the `dev` dependency group stay.
- [ ] `uv.lock` is regenerated and contains only the project and its dependency-group closure (about 15 packages)
- [ ] CI is green on the PR to `dev` (Lint & Format, Tests, direnv smoke lanes, Scaffold Drift)
- [ ] Once this reaches `main` with the next devkit train, all 14 open Dependabot alerts close, and #433-#437 are closed (Dependabot closes them itself, or by hand)

### Implementation Notes

- `uv.lock` itself stays. A Python consumer commits its lock, and the smoke repo should look like one.
- Branch off `dev`, PR to `dev`. Do not merge #433-#437 into `main`.
- Longer-term direction (vig-os/org-config#307, "Prefer Renovate"): Dependabot security updates are to be turned off org-wide in favour of Renovate. That needs this repo's Renovate to cover `pep621`, which is tracked separately: devkit's `assets/smoke-test/renovate.json` still ships `enabledManagers: ["github-actions"]`, and deploy 0.5.1 (`16f26dd`) reverted #221's broadening.

### Related Issues

Related to #221, #433, #434, #435, #436, #437, vig-os/org-config#307, vig-os/devkit#1755

### Priority

Medium

### Changelog Category

Security

### Additional Context

Minimal-payload rationale (what the smoke repo is for, and why language/setup variants belong in devkit's CI rather than here): vig-os/devkit#1762 (rendered-consumer variant matrix).

---

# [Comment #1]() by [c-vigo]()

_Posted on September 29, 2026 at 01:21 PM_

Implemented in #439, which targets `dev` and reaches `main` with the next devkit release train.

A correction to the Implementation Notes: the Renovate gap they describe is closed. vig-os/devkit#1764 broadened the smoke Renovate config, and `renovate.json` on both `dev` and `main` here already enables `["github-actions", "pep621"]`. No Renovate change is needed for this issue.

The leftover `dependabot/uv/*` branches from #433-#437 are left in place.

