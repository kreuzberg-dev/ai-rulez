______________________________________________________________________

## priority: medium

# Playwright MCP Patterns

Standards for Playwright MCP testing and browser automation at kreuzberg.dev.

## MCP Integration

- Use Playwright MCP server for browser automation tasks
- Connect via `npx @anthropic/mcp-playwright` for AI-assisted testing
- Leverage MCP for visual regression testing and screenshot comparison

## Test Structure

- Page Object Model (POM) for all test suites
- Fixtures for common setup/teardown operations
- Test files: `*.spec.ts` in `tests/e2e/` directory

## Browser Automation Patterns

- Use `page.goto()` with `waitUntil: 'networkidle'` for navigation
- Prefer `data-testid` attributes for element selection
- Use `expect(page).toHaveScreenshot()` for visual regression
- Implement retry logic for flaky network operations

## Configuration

- `playwright.config.ts` at project root
- Configure projects for chromium, firefox, webkit
- Use `baseURL` from environment variables
- Set `retries: 2` in CI, `retries: 0` locally

## CI Integration

- Run Playwright tests in dedicated CI job
- Use `playwright install --with-deps` in CI setup
- Store trace files as artifacts on failure
- Use sharding for parallel test execution: `--shard=1/4`

## Anti-Patterns

- Using CSS selectors instead of data-testid
- Not waiting for network idle before assertions
- Hardcoding URLs instead of using baseURL config
- Running browser tests without headless mode in CI
- Not storing traces/screenshots on failure
