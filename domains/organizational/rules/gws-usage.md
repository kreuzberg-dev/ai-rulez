______________________________________________________________________

## priority: medium

# GWS Usage

Standards for using Google Workspace CLI (gws) at kreuzberg.dev.

## Core Commands

- `gws drive` - Google Drive file operations (upload, download, share, list)
- `gws sheets` - Google Sheets read/write operations for data management
- `gws chat` - Google Chat messaging for team notifications and automation

## Drive Operations

- Use `gws drive upload` for artifact distribution
- Share files with team using `gws drive share --team`
- Organize files in standard folder hierarchy: `/kreuzberg-dev/{project}/{category}/`

## Sheets Integration

- Use Sheets for tracking release matrices and version compatibility
- `gws sheets read` for pulling configuration data into CI
- `gws sheets write` for updating build status dashboards

## Chat Automation

- `gws chat send` for CI/CD notifications to team channels
- Format messages with markdown for readability
- Use webhooks for automated release announcements

## Authentication

- Service account credentials stored in CI secrets
- Local development uses `gws auth login` with OAuth
- Never commit credentials; use environment variables

## Anti-Patterns

- Hardcoding file IDs (use named lookups or environment variables)
- Polling Sheets instead of using push notifications
- Sending unformatted messages to Chat
