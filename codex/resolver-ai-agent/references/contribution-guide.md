# Open Source Contribution Guide

## Table of Contents
- 1. Fork the repository
- 2. Clone your fork
- 3. Set up remotes
- 4. Create a branch
- 5. Implement changes
- 6. Commit changes
- 7. Push branch
- 8. Open pull request
- 9. Handle review
- Common mistakes
- Troubleshooting
- Submission checklist

## 1. Fork the Repository

Create a fork of the original GitHub repository to your account.

Example:
- Upstream: `https://github.com/OriginalOwner/ProjectName`
- Fork: `https://github.com/YourUsername/ProjectName`

## 2. Clone Your Fork

```bash
git clone https://github.com/YourUsername/ProjectName.git
cd ProjectName
```

Clone your fork, not upstream.

## 3. Set Up Git Remotes

```bash
git remote -v

git remote remove origin
git remote add origin https://github.com/YourUsername/ProjectName.git
git remote add upstream https://github.com/OriginalOwner/ProjectName.git

git remote -v
```

Expected contract:
- `origin` -> your fork (push target)
- `upstream` -> original repository (sync source)

## 4. Create a New Branch

```bash
git checkout -b type/description-issue-number
```

Examples:
- `feat/graceful-shutdown-151`
- `refactor/extract-components-38`
- `fix/broken-link-42`

## 5. Make Your Changes

- Read the issue carefully.
- Change only what is required.
- Follow repository style and conventions.
- Run relevant tests before commit.

## 6. Commit Your Changes

```bash
git status
git add .
git commit -m "feat: add graceful shutdown handler (#151)

- Add shutdown signal handlers
- Close database connections before exit
- Add tests for SIGTERM and SIGINT"
```

Conventional types: `feat`, `fix`, `refactor`, `docs`, `test`.

## 7. Push to Your Fork

```bash
git push origin feat/graceful-shutdown-151
```

If prompted for password over HTTPS, use a Personal Access Token.

## 8. Create the Pull Request

PR title format:

```text
type: Brief description (#issue-number)
```

PR description template:

```markdown
Closes #issue-number

## Changes
- Itemized summary of code changes

## Testing
- What was tested
- Results and edge cases checked

## Notes
- Optional context or limitations
```

Keep `Closes #issue-number` at the top to link and auto-close.

## 9. Wait for Review and Address Feedback

If review requests changes:

```bash
git add .
git commit -m "fix: address review feedback"
git push origin your-branch-name
```

Update the same branch and PR.

## Common Mistakes

- Forgetting to include `Closes #issue-number`.
- Pushing to upstream instead of fork.
- Working directly on `main`.
- Mixing unrelated changes in one PR.
- Skipping tests.
- Using vague commit messages.

## Troubleshooting

### Permission Denied on Push

```bash
git remote -v
git remote remove origin
git remote add origin https://github.com/YourUsername/ProjectName.git
git push origin your-branch-name
```

### Merge Conflict in PR

```bash
git fetch upstream
git merge upstream/main
# resolve conflicts
git add .
git commit -m "fix: resolve merge conflicts"
git push origin your-branch-name
```

### Forgot to Link Issue

Edit PR description and add `Closes #issue-number` at the top.

## Submission Checklist

- Fork created
- Fork cloned
- `origin` and `upstream` configured correctly
- Issue branch created
- Scope limited to issue requirements
- Tests run successfully
- Clear commit messages
- Branch pushed to fork
- PR title and description follow project expectations
- `Closes #issue-number` included
