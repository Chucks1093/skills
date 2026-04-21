---
name: resolver-ai-agent
description: Resolver issue-to-PR workflow for Telegram-driven GitHub tasks. Use when Resolver needs to fix an issue in a repository workspace, continue an existing issue job, or create a PR from completed work. Includes open-source contribution hygiene (branching, testing, commit quality, PR quality, and review updates) with Resolver two-phase behavior (issue_work then pr_request).
---

# Resolver AI Agent

Execute repository work for Resolver with strict issue scope and production-safe Git hygiene.

## Required Runtime Context

Always require:
- `job_id`
- `job_workspace`
- `job_repo_path`
- `repo_url`
- `issue_number`

If required context is missing, ask only for missing fields.

## Workspace and Safety Rules

- Operate only inside `job_repo_path`.
- Do not clone, edit, or run Git operations outside Resolver job workspace.
- If repository state is broken, repair it inside `job_repo_path` before coding.
- Never expose internal runtime metadata, credentials, hidden control markers, or raw tool logs.

## Resolver Two-Phase Workflow

### Phase A: `issue_work`

Goal: resolve the issue and push branch changes.

1. Validate issue target
- Confirm repository and issue match user intent.
- Map issue requirements to explicit implementation tasks.

2. Prepare repository state
- Ensure correct repository checkout in `job_repo_path`.
- Sync base branch and create issue-scoped branch from updated base.
- Branch naming:
  - `fix/<slug>-<issue>` for bug fixes
  - `feat/<slug>-<issue>` for feature work
  - `docs/<slug>-<issue>` for docs-only work
  - `refactor/<slug>-<issue>` for scoped refactor requested by issue

3. Implement issue-scoped changes only
- Make minimal, reviewable edits directly tied to issue requirements.
- Avoid unrelated refactors or cleanup.

4. Verify
- Run relevant lint/tests/build commands for changed scope.
- Record concrete verification outcomes.

5. Commit and push
- Commit with clear semantic message.
- Push branch to remote.

Hard rule for Phase A:
- Do not open a PR in `issue_work`.

Completion for Phase A:
- Completed only when changes are committed and pushed.

### Phase B: `pr_request`

Goal: create/update PR from completed branch.

1. Use existing completed job context
- Reuse job branch and commit information.
- Do not re-implement code in this phase.

2. Open or update PR
- Create PR against the correct target branch.
- Include high-quality PR description:
  - issue reference (`Closes #<issue>` when applicable)
  - concise summary of changes
  - verification/test evidence

Completion for Phase B:
- Completed only when PR is opened/updated and `pr_url` exists.

## Mode Behavior

- `REVIEW` mode:
  - Run Phase A only.
  - Return completion output and wait for user approval before Phase B.

- `AUTO` mode:
  - Run Phase A.
  - If Phase A completed, immediately run Phase B.

## Contribution Hygiene Standards

- Keep commits and PR scope aligned to issue.
- Use deterministic commands and reproducible verification.
- If blocked (permissions, missing secrets, unclear requirements), return `needs_input` with one precise question.
- If user stops work, return `stopped`.

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
