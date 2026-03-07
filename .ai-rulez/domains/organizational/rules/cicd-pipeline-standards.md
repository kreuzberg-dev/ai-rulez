______________________________________________________________________

## priority: medium

# CI/CD Pipeline Standards

**GitHub Actions. Multi-platform. Coverage enforcement.**

## Workflow Organization

- CI split by domain: ci-rust, ci-python, ci-node, ci-wasm, ci-ruby, ci-php, ci-go, ci-java, ci-elixir, ci-validate
- Linting and formatting: ruff, clippy, rubocop, biome, phpstan, golangci-lint
- Test matrix: Python (3.10, 3.12, 3.14-dev), PHP (8.2+), Rust (stable 1.75+)
- OS matrix: Linux (amd64, arm64), macOS, Windows (where applicable)

## Artifacts and Quality Gates

- Artifacts: Rust coverage -> rust-coverage.lcov, Python -> coverage.lcov
- Quality gates: zero warnings, tests pass, coverage thresholds (Rust 95%, others 80%)
- Wheel builds: separate test-wheels.yaml for PyPI distribution testing
- Version-gated: tag-based releases trigger multi-platform builds

## Standards

- Workflows use `task` commands exclusively (never direct scripts)
- Always set `BUILD_PROFILE=ci` in GitHub Actions
- Pre-commit hooks run in validate stage
- Stages: Validate -> Build -> Test -> Deploy
