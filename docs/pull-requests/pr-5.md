---
type: pull_request
state: closed (merged)
branch: dev → main
created: 2026-02-25T09:27:12Z
updated: 2026-02-26T12:05:23Z
author: c-vigo
author_url: https://github.com/c-vigo
url: https://github.com/vig-os/devkit-smoke-test/pull/5
comments: 2
labels: none
assignees: c-vigo
milestone: none
projects: none
merged: 2026-02-25T18:02:02Z
synced: 2026-07-10T15:45:58.082Z
---

# [PR 5](https://github.com/vig-os/devkit-smoke-test/pull/5) ci: add repository_dispatch listener stub

## Description

Adds a `repository_dispatch` workflow stub to support testing cross-repository dispatch events, while bringing `main` up to date with current bootstrap, CI validation, and project documentation updates already present on `dev`.

## Type of Change

- [ ] `feat` -- New feature
- [ ] `fix` -- Bug fix
- [ ] `docs` -- Documentation only
- [ ] `chore` -- Maintenance task (deps, config, etc.)
- [ ] `refactor` -- Code restructuring (no behavior change)
- [ ] `test` -- Adding or updating tests
- [x] `ci` -- CI/CD pipeline changes
- [ ] `build` -- Build system or dependency changes
- [ ] `revert` -- Reverts a previous commit
- [ ] `style` -- Code style (formatting, whitespace)

### Modifiers

- [ ] Breaking change (`!`) -- This change breaks backward compatibility

## Changes Made

- `.github/workflows/repository-dispatch.yml`
  - Adds a dedicated workflow listening for `repository_dispatch`
  - Logs dispatch metadata/payload to validate trigger behavior
- `.devcontainer/scripts/setup-gh-repo.sh`, `.github/actions/setup-env/action.yml`, `.devcontainer/docker-compose.yml`, `.devcontainer/justfile.base`, `uv.lock`
  - Aligns bootstrap and setup flow with CI expectations
- `.pre-commit-config.yaml`
  - Removes unavailable hooks to keep CI checks consistent
- `README.md`, `docs/issues/issue-1.md`, `docs/pull-requests/pr-2.md`
  - Expands project documentation and traceability artifacts
- `CHANGELOG.md`
  - Adds Unreleased entries for issue-tracked workflow/bootstrap updates

## Changelog Entry

### Added

- Deployed initial project scaffold
- Created [README.md](./README.md)
- **Bootstrap scripts and lock state aligned for CI validation** ([#1](https://github.com/vig-os/devcontainer-smoke-test/issues/1))
  - Updated repository bootstrap/setup behavior to match the current development and CI flow
  - Refreshed pre-commit and dependency lock state to keep validation runs consistent
- **repository_dispatch listener stub** ([#3](https://github.com/vig-os/devcontainer-smoke-test/issues/3))
  - Added a dedicated workflow that listens for `repository_dispatch` events
  - Logs dispatch payload metadata as a minimal foundation for later cross-repo integration

## Testing

- [x] Tests pass locally (`just test-pytest`)
- [ ] Manual testing performed (describe below)

### Manual Testing Details

N/A

## Checklist

- [ ] My code follows the project's style guidelines
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have updated the documentation accordingly (edit `docs/templates/`, then run `just docs`)
- [x] I have updated `CHANGELOG.md` in the `[Unreleased]` section (and pasted the entry above)
- [ ] My changes generate no new warnings or errors
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
- [ ] Any dependent changes have been merged and published

## Additional Notes

Goal for this PR is to test the dispatch workflow path against `main`.

Refs: #1, #3, [devcontainer#170](https://github.com/vig-os/devcontainer/issues/170)



---
---

## Review Threads (1)

### Review by [@Copilot](https://github.com/apps/copilot-pull-request-reviewer)

_Posted on February 25, 2026 at 09:38 AM_

_File: [`.devcontainer/scripts/setup-gh-repo.sh (line 49 RIGHT)`](https://github.com/vig-os/devkit-smoke-test/pull/5#discussion_r2851904535)_

```diff
@@ -28,3 +29,28 @@ else
 	echo "✗ Could not update repo settings (insufficient permissions?)"
 	echo "  Manual setup: gh api repos/$REPO -X PATCH -f merge_commit_title=PR_TITLE -f merge_commit_message=PR_BODY -F allow_auto_merge=true"
 fi
+
+# Detach any org-level default code security configuration from this repo.
+ORG="${REPO%%/*}"
+REPO_ID=$(gh api "repos/$REPO" --jq '.id' 2>/dev/null || echo "")
+
+if [ -z "$REPO_ID" ]; then
+	echo "✗ Could not determine repo ID — skipping security config detachment"
+else
+	CONFIG_STATUS=$(gh api "repos/$REPO/code-security-configuration" --jq '.status // empty' 2>/dev/null || echo "")
+	if [ -z "$CONFIG_STATUS" ] || [ "$CONFIG_STATUS" = "detached" ]; then
+		echo "✓ No security config attached — nothing to detach"
+	else
+		echo "Detaching default code security configuration (status: $CONFIG_STATUS)..."
+		if gh api "orgs/$ORG/code-security/configurations/detach" \
+			-X DELETE \
+			--input - <<-JSON >/dev/null 2>&1; then
+{"selected_repository_ids":[$REPO_ID]}
+JSON
```

The heredoc syntax is incorrect. In bash, when using `<<-JSON`, the heredoc content and closing delimiter must be structured properly. The redirection `>/dev/null 2>&1` and conditional `; then` cannot appear on line 47 after the heredoc opener. This will cause a syntax error when the script is executed.

The heredoc should be restructured so that the command, heredoc content, and redirections are properly separated. For example, the entire `gh api` command with its heredoc should complete before the redirection and conditional check.
```suggestion
			--input - <<-JSON >/dev/null 2>&1
{"selected_repository_ids":[$REPO_ID]}
JSON
			then
```

Conversation:

- **[@c-vigo](https://github.com/c-vigo)** on February 25, 2026 at 09:43 AM — [link](https://github.com/vig-os/devkit-smoke-test/pull/5#discussion_r2851927957)

  Thanks for the review. This is actually valid bash — placing `>/dev/null 2>&1; then` on the same line as a heredoc opener is standard POSIX/bash syntax. Bash defers reading the heredoc body from subsequent lines while still parsing the `;` and `then` on the opener line. Confirmed with `bash -n`: no syntax errors.


---
---

## Commits

### Commit 1: [52d196e](https://github.com/vig-os/devkit-smoke-test/commit/52d196e106b7795a9779489f9723fd6b4aebec19) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:10 PM
docs: create README with project purpose and scope for devcontainer smoke test, 40 files modified (README.md)

### Commit 2: [c080dd5](https://github.com/vig-os/devkit-smoke-test/commit/c080dd53a8ce5bb143694d5800d39da1e00603cb) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:18 PM
chore: update uv.lock, 108 files modified (uv.lock)

### Commit 3: [003a545](https://github.com/vig-os/devkit-smoke-test/commit/003a545e4d10135c93883b33d66d0997e248ebcf) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:48 PM
chore: update devcontainer image and template, 41 files modified (.devcontainer/scripts/setup-gh-repo.sh, .pre-commit-config.yaml)

### Commit 4: [a10e73c](https://github.com/vig-os/devkit-smoke-test/commit/a10e73cf0cd6ddb03b22df5231bd35030fb8e284) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 03:52 PM
docs: update CHANGELOG, 4 files modified (CHANGELOG.md)

### Commit 5: [bdb2f5c](https://github.com/vig-os/devkit-smoke-test/commit/bdb2f5c1c41bb520344c520acad69ddf75f209cf) by [c-vigo](https://github.com/c-vigo) on February 24, 2026 at 04:11 PM
feat: install pre-commit in setup action, 9 files modified (.github/actions/setup-env/action.yml)

### Commit 6: [925e63a](https://github.com/vig-os/devkit-smoke-test/commit/925e63ab5fb8314af26f7e57cf7fdc7b5616c4d4) by [commit-action-bot[bot]](https://github.com/apps/commit-action-bot) on February 25, 2026 at 04:21 AM
chore: sync issues and PRs, 139 files modified (docs/issues/issue-1.md, docs/pull-requests/pr-2.md)

### Commit 7: [3d37608](https://github.com/vig-os/devkit-smoke-test/commit/3d37608dff583fb2a27872d6eb2b6e35113f807b) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 07:27 AM
chore: remove pre-commit hooks that are unavalable in CI, 53 files modified (.pre-commit-config.yaml)

### Commit 8: [bc75ec6](https://github.com/vig-os/devkit-smoke-test/commit/bc75ec602e139f9c535357217826009c90667f74) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:12 AM
chore: update devcontainer image and template, 15 files modified (.devcontainer/docker-compose.yml, .devcontainer/justfile.base, uv.lock)

### Commit 9: [356dc0d](https://github.com/vig-os/devkit-smoke-test/commit/356dc0d5d5dc73c70fb78555b0d68d53a09ee4d9) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:26 AM
chore: align setup scripts and lockfile for CI validation (#2), 270 files modified (.devcontainer/docker-compose.yml, .devcontainer/justfile.base, .devcontainer/scripts/setup-gh-repo.sh, .github/actions/setup-env/action.yml, .pre-commit-config.yaml, CHANGELOG.md, README.md, uv.lock)

### Commit 10: [1a9e1d5](https://github.com/vig-os/devkit-smoke-test/commit/1a9e1d52a9a2b925b8b849f7e53f0b12bbd2aeb9) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:36 AM
feat(ci): add repository_dispatch listener stub, 28 files modified (.github/workflows/repository-dispatch.yml)

### Commit 11: [2dfe086](https://github.com/vig-os/devkit-smoke-test/commit/2dfe0864c3b8250c3efd7d4c9adca6b086518315) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:41 AM
docs(changelog): add unreleased entry for dispatch listener, 3 files modified (CHANGELOG.md)

### Commit 12: [11c1bb2](https://github.com/vig-os/devkit-smoke-test/commit/11c1bb2809dc38d3a913f166f56377ce955dd4cd) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:49 AM
docs(changelog): add entry for bootstrap scripts, 4 files modified (CHANGELOG.md)

### Commit 13: [f0861a1](https://github.com/vig-os/devkit-smoke-test/commit/f0861a1f17721b90a617a4b5afdbb6b0accaaabb) by [c-vigo](https://github.com/c-vigo) on February 25, 2026 at 08:59 AM
ci: add repository_dispatch listener stub (#4), 35 files modified (.github/workflows/repository-dispatch.yml, CHANGELOG.md)
