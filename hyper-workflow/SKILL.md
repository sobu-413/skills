---
name: hyper-workflow
description: Standard workflow for coding agents working in git repos — scoped changes, verification, Conventional Commits, branch naming, and pull requests for every change. Use for any implement, fix, refactor, test, docs, review-response, commit, push, or PR task, even small ones.
metadata:
  short-description: Standardize coding, commits, and PRs
---

# Hyper Workflow
Every change follows **orient → branch → implement → verify → commit → open PR → wait for approval → merge**. Every change ships through a pull request, including small, docs-only, and solo-repo changes; a request to "push" means push a branch and open a PR. Repository conventions (CONTRIBUTING, CLAUDE.md, AGENTS.md, CI config) override these defaults where they conflict, but never the hard rules.

## 1. Orient
Run `git status`; if the tree has changes you didn't make, stop and ask. Sync the default branch with `git fetch origin && git checkout main && git pull --ff-only`. Find how the repo runs format, lint, type-check, tests, and build; CI config is the source of truth for what must pass. Ask a question only when the answer would materially change the solution; otherwise state a brief, reversible assumption.

## 2. Branch
Create a branch from the synced default branch:
```text
<type>/<short-kebab-description>
```
Use the commit types below, e.g. `feat/csv-export`, `fix/null-session-crash`, `docs/setup-guide`. Keep it under ~40 characters; prefix an issue number when one exists, e.g. `fix/123-null-session-crash`.

## 3. Implement
One concern per PR: it should be explainable in one sentence. If the work splits naturally (a refactor that enables a feature), ship the refactor as its own PR first. Match surrounding naming, structure, and idiom. No drive-by changes: don't reformat untouched code or rename unrelated symbols. Log unrelated issues under "Noticed, not touched" in the PR instead of fixing them. Add tests for new behavior and a failing-then-passing test for bug fixes.

## 4. Verify
All gates are required before opening a PR: tests pass, lint and type-check clean with zero new warnings, and build succeeds. Don't silence rules (`eslint-disable`, `@ts-ignore`) without justifying each in the PR. Self-review the full diff with `git diff main...HEAD` as a reviewer would and remove debug output, stray files, and unintended edits. For UI changes, capture before/after screenshots or list the exact pages and states a human should check. If a gate fails and you can't fix it, open the PR as a draft and state what's failing; never claim success you have not observed.

## 5. Commit
Use Conventional Commits:
```text
<type>(<optional scope>): <imperative summary, lowercase, ≤72 chars, no period>

<optional body: why the change was made, not what the diff shows>

<optional footer: Closes #123, BREAKING CHANGE: ...>
```
Types: `feat`, `fix`, `refactor`, `perf`, `test`, `docs`, `style`, `build`, `ci`, `chore`, `revert`. Mark breaking changes with `feat!:` or a `BREAKING CHANGE:` footer. Commit in logical steps that each build. Stage files explicitly when unrelated changes exist.

## 6. Open the PR
```bash
git push -u origin HEAD
gh pr create --title "<conventional commit title>" --body-file <file> [--draft]
```
The title follows commit format because it becomes the squash commit. Fill every section of this body, writing "N/A" when empty:
```markdown
## Summary
One or two sentences on what changed and why.

## Changes
- Key change, grouped by area.

## Verification
- Commands run and their results; manual checks performed.

## Noticed, not touched
- file:line — one-line description of unrelated issues.

## Notes
Risks, limitations, follow-ups, screenshots for UI changes.

Closes #123
```
Report the PR link only after confirming it exists.

## 7. Review and merge
Check status with `gh pr view --json reviewDecision,statusCheckRollup`. Merge only when the user explicitly asks or `reviewDecision` is `APPROVED`, and all checks pass. Approval is per PR and never carries over. Merge with `gh pr merge --squash --delete-branch` unless the repo prefers another method. Address review comments with new commits and reply to each with what changed or why you disagree. If `main` moved and conflicts appear, rebase or merge it in, re-run all gates, and note it in the PR.

## Hard rules
- Never push directly to the default branch.
- Never merge without explicit approval.
- Never force-push a branch someone else has reviewed or committed to.
- Never skip hooks (`--no-verify`), bypass signing, or enable auto-merge without explicit permission.
- Never commit secrets, `.env` files, or credentials; if one is already committed, flag it and stop.
- Never delete or skip failing tests to make CI green.

## Report
End with a short summary: what changed, gates run and their outcomes, branch and PR links, and anything unresolved.
