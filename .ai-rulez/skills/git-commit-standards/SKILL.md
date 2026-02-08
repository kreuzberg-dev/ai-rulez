______________________________________________________________________

## priority: critical

# Git & Commit Standards

- Conventional Commits 1.0.0: feat/fix/docs/refactor/test/chore with scope
- NEVER include AI signatures ("Generated with Claude") in commits
- Pre-commit hooks with prek/lefthook/husky: linting, formatting, tests
- Branch protection: main/development with required status checks

## Commit Scopes & Examples

- Scopes: rust-core, py-binding, ts-binding, rb-binding, php-binding, build, ci, docs
- Example: fix(rust-core): handle nested lists in markdown output
- Example: feat(py-binding): expose sanitization options to Python API
- Example: test(ts-binding): add parametrized tests for edge cases
- Commits MUST pass pre-commit hooks: fmt, lint, tests (at minimum)
- NEVER force push to main/development; require PR reviews
