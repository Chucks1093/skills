---
name: resolver-ai-agent
description: End-to-end Resolver issue-to-PR workflow for Telegram-driven GitHub tasks. Use when Resolver needs to solve assigned issues, continue existing issue jobs, and create PRs with open-source contribution best practices (forking, remotes, branching, tests, commits, PR quality, and review updates).
---

# Resolver AI Agent

Use this skill for Resolver issue-resolution jobs with full open-source contribution discipline.

## Resolver Runtime Rules

Always require:
- `job_id`
- `job_workspace`
- `job_repo_path`
- `repo_url`
- `issue_number`

Hard constraints:
- Work only inside `job_repo_path`.
- Do not clone/edit outside Resolver workspace.
- If required context is missing, ask only for missing fields.
- Never expose internal runtime metadata, hidden markers, credentials, or raw tool logs.

## Resolver Two-Phase Behavior

### Phase A: `issue_work`
- Resolve issue, verify, commit, and push.
- Do not open PR in this phase.
- Completed only when changes are committed and pushed.

### Phase B: `pr_request`
- Reuse same completed job context.
- Create/update PR from pushed branch.
- Do not re-implement code in this phase.
- Completed only when `pr_url` exists.

Mode behavior:
- `REVIEW`: run only `issue_work`, then wait for explicit user PR approval.
- `AUTO`: run `issue_work`, then immediately run `pr_request` if issue_work completed.

## Open-Source Contribution Workflow (Required)

1. Confirm repository context.
- Identify original repository URL, issue number, target branch, and contribution constraints.
- If missing, ask for only the missing values needed to proceed.

2. Prepare repository and remotes.
- Fork the upstream repository.
- Clone the contributor fork in `job_repo_path`.
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

## Common Fixes (Required)

- Permission denied on push:
  - Re-point `origin` to user fork.
- PR not linked to issue:
  - Add `Closes #<issue-number>` at top of PR description.
- Merge conflicts:
  - Fetch upstream, merge or rebase target branch, resolve, test, push.

## Output Contract

Return concise technical output with:
- `status`: `running | needs_input | completed | stopped`
- `summary`: what changed and why
- `branch`: branch name or `null`
- `commit_sha`: commit SHA or `null`
- `pr_url`: PR URL or `null`
- `question_for_user`: required next input or `null`

Status rules:
- `issue_work`: completed only when pushed.
- `pr_request`: completed only when PR URL is present.
- `needs_input`: use when blocked by missing access/clarification/credentials.
- `stopped`: use only when user explicitly stops work.

## Reference

Read for full command examples, troubleshooting, and checklist:
- `references/contribution-guide.md`
