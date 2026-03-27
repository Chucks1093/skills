---
name: open-source-contribution
description: End-to-end workflow for contributing to open source on GitHub, from forking and syncing remotes through branching, commits, pull requests, review feedback, and merge-conflict resolution. Use when asked to solve an issue in an external repository, open a PR, fix contribution mistakes, or follow contribution best practices.
---

# Open Source Contribution

## Overview

Use this skill to execute issue-to-PR contribution workflows with clean Git hygiene and GitHub-ready output.

Keep guidance actionable and repository-specific. Favor minimal, issue-scoped changes.

## Workflow

1. Confirm repository context.
- Identify original repository URL, issue number, target branch, and contribution constraints.
- If missing, ask for only the missing values needed to proceed.

2. Prepare repository and remotes.
- Fork the upstream repository.
- Clone the contributor fork.
- Ensure remotes follow this contract:
  - `origin` -> contributor fork
  - `upstream` -> original repository

3. Create an issue-scoped branch.
- Create a branch from updated base branch (usually `main`).
- Use naming: `feat/<slug>-<issue>`, `fix/<slug>-<issue>`, `refactor/<slug>-<issue>`, `docs/<slug>-<issue>`.

4. Implement only required changes.
- Map requirements from issue text to explicit code changes.
- Avoid unrelated refactors.
- Run project tests/lint or targeted verification.

5. Commit with clear semantics.
- Write conventional commit subject with issue reference when requested.
- Keep commit message specific to behavior and tests performed.

6. Push and open pull request.
- Push branch to `origin`.
- Create PR against `upstream` target branch.
- Ensure PR description starts with `Closes #<issue-number>` when appropriate.
- Include `Changes` and `Testing` sections.

7. Address review feedback.
- Apply requested updates on the same branch.
- Re-test.
- Push follow-up commits.

## Output Requirements

When helping a user, provide:
- Exact commands for their current step.
- A short checklist of completion criteria.
- Copy-ready commit message and PR template when requested.

## Common Fixes

- Permission denied on push:
  - Re-point `origin` to user fork.
- PR not linked to issue:
  - Add `Closes #<issue-number>` at top of PR description.
- Merge conflicts:
  - Fetch upstream, merge or rebase target branch, resolve, test, push.

## Reference

For full guidance, examples, and troubleshooting details, read:
- `references/contribution-guide.md`
