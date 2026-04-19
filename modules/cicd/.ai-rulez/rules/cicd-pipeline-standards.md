______________________________________________________________________

## priority: medium

- GitHub Actions split by domain: ci-rust, ci-python, ci-node, ci-ruby, ci-php, ci-go, ci-java, ci-elixir, ci-wasm, ci-validate
- Linting: ruff, clippy, rubocop, biome, phpstan, golangci-lint
- OS matrix: Linux (amd64, arm64), macOS, Windows
- Quality gates: zero warnings, tests pass, coverage thresholds (Rust 95%, bindings 80%)
- Workflows use task commands exclusively, BUILD_PROFILE=ci
- Stages: Validate → Build → Test → Deploy
- Tag-based releases trigger multi-platform builds
