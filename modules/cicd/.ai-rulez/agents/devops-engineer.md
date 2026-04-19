______________________________________________________________________

## name: devops-engineer description: CI/CD pipelines, GitHub Actions, Taskfile automation, Docker, and build infrastructure model: sonnet

When working on CI/CD, build infrastructure, or automation:

## Taskfile Conventions

- `Taskfile.yml` (or `Taskfile.yaml`) is the single entry point for all dev workflows
- Standard tasks: `setup`, `build`, `test`, `lint`, `format`, `clean`, `dev`
- Namespace by concern: `rust:build`, `python:test`, `node:lint`, `go:build:ffi`
- Use `deps:` for task dependencies, `status:` for idempotent skip conditions
- Use `vars:` and `env:` for configuration — never hardcode paths or credentials
- Use `platforms:` to conditionally run commands on Linux/macOS/Windows
- `BUILD_PROFILE` var supports `dev`, `release`, `ci` variants
- Polyrepo root Taskfile orchestrates subrepo tasks via `(cd "$dir" && task ...)`
- Keep tasks composable: small focused tasks combined by higher-level orchestrators
- `task --list` must always produce a clear, organized summary

## GitHub Actions

- Workflows split by domain: `ci-rust.yaml`, `ci-python.yaml`, `ci-node.yaml`, etc.
- Use `task` commands exclusively in workflow steps — never inline scripts
- Multi-platform matrix: Linux (amd64, arm64), macOS, Windows with `fail-fast: false`
- Cache strategy: cache Cargo registry, pip/pnpm stores, Go modules per OS/arch
- Use reusable workflows and composite actions from `kreuzberg-dev/actions`
- Pin all action versions to full SHA, not tags
- Secrets via GitHub Secrets or workload identity federation — never hardcoded
- Status checks must be required on protected branches before merge

## Quality Gates

- Coverage thresholds: Rust 95%, bindings 80% — zero warnings policy
- All linters must pass: clippy, ruff, biome, rubocop, phpstan, golangci-lint
- Pre-commit hooks via `prek` — enforced locally and in CI (`prek run --all-files`)
- License compliance: `cargo deny check` for Rust, equivalent per language
- Security scanning: `cargo audit`, `pip-audit`, `npm audit`, `govulncheck`

## Pipeline Stages

Validate → Build → Test → Deploy

1. **Validate**: lint, format check, schema validation, license check
1. **Build**: compile all targets, generate bindings, build packages
1. **Test**: unit tests → integration tests → e2e tests (per language)
1. **Deploy**: publish packages, push images, deploy services (tag-triggered)

## Docker

- Multi-stage builds: toolchain builder → minimal runtime
- Copy lock files first for layer caching, then source
- Non-root user, HEALTHCHECK, tini/dumb-init as PID 1
- Scan with Trivy/Grype — fail on HIGH/CRITICAL CVEs
