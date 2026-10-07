---
name: hyper-workflow
description: Standard workflow for coding agents working in git repos — scoped changes, verification, Conventional Commits, branch naming, and consistent pull requests. Use for any implement, fix, refactor, test, docs, review-response, commit, push, or PR task, even small ones.
metadata:
  short-description: Standardize coding, commits, and PRs
---

# Hyper Workflow
Follow this workflow for any change that touches a git repository. It keeps changes small, verified, and easy to review. Repository conventions (CONTRIBUTING, CLAUDE.md, AGENTS.md, CI config, existing history) override the defaults below.

## 1. Orient
Read the request and restate the goal in one sentence. Run `git status` and check the current branch, uncommitted changes, and recent commits. Locate the relevant code, tests, and conventions before deciding on an approach. Ask a question only when the answer would materially change the solution; otherwise state a brief, reversible assumption.

## 2. Branch
Never commit directly to the default branch. Create a branch from an up-to-date base:
```text
<type>/<short-kebab-description>
```
Use the same types as commits, e.g. `feat/csv-export`, `fix/null-session-crash`, `docs/setup-guide`. Prefix with an issue number when one exists: `fix/123-null-session-crash`.

## 3. Implement
Make the smallest coherent change that achieves the goal. Match surrounding naming, structure, comment density, and idiom. Do not reformat untouched code, rename unrelated symbols, or bundle drive-by refactors; note them as follow-ups instead. Add or update tests for changed behavior. Never commit secrets, credentials, large binaries, or generated artifacts unless the repo already tracks them.

## 4. Verify
Run the checks the repo provides, in this order where they exist: format, lint, type-check, tests, build. Prefer targeted tests first, then the full suite when practical. Inspect the full diff with `git diff` before committing and remove debug output, stray files, and unintended edits. If a check fails, fix it or report the failure honestly with its output; never claim success you have not observed.

## 5. Commit
Use Conventional Commits:
```text
<type>(<optional scope>): <imperative summary, ≤72 chars, no period>

<optional body: why the change was made, not what the diff shows>

<optional footer: Closes #123, BREAKING CHANGE: ...>
```
Types: `feat`, `fix`, `refactor`, `perf`, `test`, `docs`, `style`, `build`, `ci`, `chore`, `revert`. Keep each commit focused on one logical change. Stage files explicitly rather than with blanket adds when unrelated changes exist. Prefer new commits over amending pushed history.

## 6. Pull request
Push and open a PR only when requested or already authorized. Title it like a commit subject. Use this description template, omitting empty sections:
```markdown
## Summary
One or two sentences on what changed and why.

## Changes
- Key change, grouped by area.

## Verification
- Commands run and their results; manual checks performed.

## Notes
Risks, limitations, follow-ups, screenshots for UI changes.

Closes #123
```
Keep PRs small enough to review in one sitting; split unrelated work into separate PRs. Create PRs with `gh pr create` and report the link only after confirming it exists.

## 7. Review and merge
Respond to every review comment with a fix or a reasoned reply, pushing fixes as new commits. Re-run verification after each round. Merge only when asked, after CI passes and required approvals exist. Use the repo's preferred merge method (squash by default for single-purpose PRs) and delete the branch afterward. Never force-push shared branches, skip hooks, bypass signing, or enable auto-merge without explicit permission.

## Report
End with a short summary: what changed, checks run and their outcomes, branch/commit/PR links, and any unresolved issues or follow-ups.
