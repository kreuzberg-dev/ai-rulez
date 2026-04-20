---
priority: medium
---

# Pre-commit Tooling

Shared hooks run via `prek run --all-files`. Each repo's `.pre-commit-config.yaml` defines its hooks.

| Scope | Tools |
|-------|-------|
| **General** | trailing-whitespace, end-of-file-fixer, check-merge-conflict, detect-private-key, typos |
| **Rust** | cargo-fmt, cargo-clippy (-D warnings), cargo-machete, cargo-deny, cargo-sort |
| **Python** | ruff (lint+format), mypy |
| **JS/TS** | biome (format+lint), oxlint |
| **Go** | golangci-lint |
| **Java** | cpd, checkstyle, maven verify |
| **Ruby** | rubocop (format+lint), steep |
| **C#** | dotnet format |
| **PHP** | php-cs-fixer, phpstan |
| **Elixir** | mix credo, mix format |
| **R** | styler, lintr |
| **C/C++** | clang-format, cppcheck |
| **Shell** | shfmt, shellcheck |
| **TOML** | taplo, pyproject-fmt |
| **Markdown** | rumdl-fmt, textlint (docs/) |
| **Git** | gitfluff (commit msg linting), ai-rulez-generate |
| **GH Actions** | actionlint |
| **Helm/K8s** | helm-lint, kubeconform |
