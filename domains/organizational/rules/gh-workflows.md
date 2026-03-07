______________________________________________________________________

## priority: medium

# GitHub CLI Workflows

Standards for using GitHub CLI (gh) for PR management, issue tracking, and workflow automation.

## Pull Request Workflow

- Create PRs with `gh pr create --title "..." --body "..."`
- Use PR templates from `.github/PULL_REQUEST_TEMPLATE.md`
- Review PRs with `gh pr review --approve` or `gh pr review --request-changes`
- Merge with `gh pr merge --squash` (preferred) or `--rebase`

## Issue Management

- Create issues with `gh issue create --label "..." --assignee "@me"`
- Use issue templates from `.github/ISSUE_TEMPLATE/`
- Link issues to PRs with `Fixes #123` in PR body
- Triage with `gh issue list --label "needs-triage"`

## Workflow Automation

- Monitor CI with `gh run list` and `gh run view`
- Re-run failed jobs with `gh run rerun --failed`
- Trigger workflows manually with `gh workflow run`
- View workflow logs with `gh run view --log`

## Release Management

- Create releases with `gh release create v1.2.3 --generate-notes`
- Upload artifacts with `gh release upload v1.2.3 ./dist/*`
- Draft releases for pre-release validation

## PR Template Standards

- Summary section with bullet points
- Test plan with checklist
- Breaking changes section when applicable
- Link to related issues

## Anti-Patterns

- Manual GitHub web UI operations for repeatable tasks
- Not linking PRs to issues
- Force-merging without CI checks passing
- Skipping PR description/template
