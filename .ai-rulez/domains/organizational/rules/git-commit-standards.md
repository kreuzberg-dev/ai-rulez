______________________________________________________________________

## priority: high

# Git Commit Standards

**Human-only authorship. No AI signatures or tool mentions in commits.**

## Authorship Policy

- All commits must appear to be authored by humans only
- NEVER include AI signatures like "Generated with Claude Code" in commit messages
- NEVER add "Co-Authored-By: Claude" or similar AI tool credits
- NEVER mention AI tools (Claude, ChatGPT, Copilot, etc.) in commit messages
- Commits must represent genuine human authorship and decision-making

## Conventional Commits

- Format: `type(scope): description`
- Types: feat, fix, docs, refactor, test, chore
- Scopes: rust-core, py-binding, ts-binding, rb-binding, php-binding, build, ci, docs
- Example: `fix(rust-core): handle nested lists in markdown output`
- Example: `feat(py-binding): expose sanitization options to Python API`

## Branch and Hook Policy

- Pre-commit hooks with prek/lefthook/husky: linting, formatting, tests
- Branch protection: main/development with required status checks
- NEVER force push to main/development; require PR reviews
- Commits MUST pass pre-commit hooks: fmt, lint, tests (at minimum)
