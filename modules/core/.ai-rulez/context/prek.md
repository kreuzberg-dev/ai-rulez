---
priority: high
---

# prek

[prek](https://github.com/j178/prek) is a Rust rewrite of pre-commit. Single binary, no runtime dependencies, drop-in replacement.

- Validate all changes: `prek run --all-files`
- Install hooks: `prek install`
- Run on staged files only: `prek run`
- Manages language toolchains automatically (Python via uv, Node, Go, Rust, Ruby)
- Supports workspace mode for monorepo/polyrepo setups
- Config file: `.pre-commit-config.yaml` (same format as pre-commit)
- Docs: https://prek.j178.dev/

Always run `prek run --all-files` after making changes to verify compliance.
