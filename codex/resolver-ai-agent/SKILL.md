---
name: resolver-ai-agent
description: End-to-end Resolver issue-to-PR workflow for Telegram-driven GitHub tasks. Use when Resolver needs to solve an issue in a repository workspace, continue an existing job, create a PR from completed work, follow open-source contribution best practices (forking, remotes, branch hygiene, tests, commits, PR quality, review updates), and return structured job outcomes.
---

# Resolver AI Agent

Use this skill to execute issue-to-PR workflows with open-source contribution quality, adapted to Resolver runtime and two-phase behavior.

Keep changes actionable, issue-scoped, and repository-specific.

## Resolver Runtime Contract

Always require:
- `job_id`
- `job_workspace`
- `job_repo_path`
- `repo_url`
- `issue_number`

If required context is missing, ask only for missing values.

Hard constraints:
- Work only inside `job_repo_path`.
- Do not clone/edit outside Resolver job workspace.
- Do not expose internal runtime metadata, credentials, hidden markers, or raw tool logs.

## Two-Phase Resolver Behavior

### Phase A: `issue_work`

Objective: resolve the issue and push branch changes.

- Create/repair repo state in `job_repo_path`.
- Implement issue-scoped changes.
- Verify with tests/lint/build.
- Commit and push branch.
- Do not open PR in this phase.

Completion rule:
- `issue_work` is completed only when changes are committed and pushed.

### Phase B: `pr_request`

Objective: create/update PR from already pushed work.

- Reuse same job/repo/branch context.
- Open or update PR.
- Include high-quality PR description and issue linkage.
- Do not re-implement issue code changes in this phase.

Completion rule:
- `pr_request` is completed only when `pr_url` exists.

## Mode Behavior

- `REVIEW` mode:
  - Run `issue_work` only.
  - Return completion output and wait for explicit user PR approval.

- `AUTO` mode:
  - Run `issue_work`.
  - If completed, immediately run `pr_request`.

## Open-Source Contribution Workflow (Full)

1. Confirm repository context.
- Identify original repository URL, issue number, target branch, and constraints.
- Ask only for missing values needed to proceed.

2. Prepare repository and remotes.
- Fork upstream when required by repo contribution policy.
- Clone contributor fork in `job_repo_path`.
- Ensure remotes contract:
  - `origin` -> contributor fork (push target)
  - `upstream` -> original repository (sync source)

3. Create issue-scoped branch from updated base.
- Update base branch first (usually `main`).
- Branch naming:
  - `feat/<slug>-<issue>`
  - `fix/<slug>-<issue>`
  - `refactor/<slug>-<issue>`
  - `docs/<slug>-<issue>`

4. Implement only required changes.
- Map issue requirements to explicit code changes.
- Avoid unrelated refactors.
- Keep diff minimal and reviewable.

5. Verify changes.
- Run relevant lint/tests/build (targeted where possible).
- Record concrete verification evidence.

6. Commit with clear semantics.
- Use clear commit subject and body.
- Keep commit message specific to behavior and verification.
- Use conventional type when repository expects it.

7. Push branch.
- Push to `origin` branch.
- Ensure pushed branch matches issue scope.

8. Open pull request (`pr_request` phase).
- Create PR from contributor branch to upstream target branch.
- Use quality PR content:
  - `Closes #<issue-number>` when appropriate
  - `Changes` section
  - `Testing` section
  - optional `Notes`

9. Handle review feedback.
- Apply requested updates on same branch.
- Re-test.
- Push follow-up commits.

## Common Fixes

- Permission denied on push:
  - Re-point `origin` to contributor fork.
- PR not linked to issue:
  - Add `Closes #<issue-number>` at top of PR description.
- Merge conflicts:
  - Fetch upstream, merge/rebase target branch, resolve, test, push.

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

For full command examples, troubleshooting details, and checklist, read:
- `references/contribution-guide.md`
