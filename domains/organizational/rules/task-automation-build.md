______________________________________________________________________

## priority: medium

# Task Automation and Build

**Taskfile.yaml for all workflows. setup -> build -> test -> lint**

## Task Runner Standards

- Taskfile.yaml is the primary interface for all development tasks
- `task setup`: install all language deps (Python, Ruby, PHP, JS, Go, Java, C#, Elixir)
- `task build`: compile Rust core + JavaScript bindings
- `task test`: run all language test suites (pytest, cargo test, rspec, vitest, phpunit)
- `task lint`: ruff + clippy + phpstan + rubocop + golangci-lint + more
- `task format`: ruff fix + cargo fmt + rubocop --autocorrect + biome + phpcbf
- `task cov:all`: generate Rust + Python coverage reports (lcov format)
- `task bench`: benchmark harness via tools/benchmark-harness

## Build Configuration

- Environment variables in Taskfile (RUST_LOG, RUBY_BIN, BUNDLER_VERSION)
- Lock files committed: uv.lock, pnpm-lock.yaml, go.sum, Cargo.lock, composer.lock
- BUILD_PROFILE supports dev/release/ci variants

## Anti-Patterns

- Manual commands instead of task tasks
- Hardcoded paths (use Taskfile variables)
- Skipping lock file commits
- Manual version bumps (use sync_versions)
