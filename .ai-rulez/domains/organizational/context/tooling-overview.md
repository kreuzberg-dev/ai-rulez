______________________________________________________________________

## priority: medium

# Tooling Overview

Overview of CLI tools and automation used across kreuzberg.dev projects.

## Core Tools

| Tool | Purpose | Key Commands |
|------|---------|-------------|
| **RTK** | Rust workspace toolkit | `rtk init -g`, `rtk build`, `rtk sync`, `rtk release` |
| **task** | Task runner (go-task) | `task setup`, `task build`, `task test`, `task lint` |
| **gh** | GitHub CLI | `gh pr create`, `gh issue create`, `gh run view` |
| **gcloud** | Google Cloud CLI | `gcloud run deploy`, `gcloud builds submit` |
| **gws** | Google Workspace CLI | `gws drive`, `gws sheets`, `gws chat` |
| **prek** | Pre-commit hooks | `prek install`, `prek run` |

## Browser and Testing Tools

| Tool | Purpose | Key Commands |
|------|---------|-------------|
| **Playwright MCP** | Browser automation | `npx @anthropic/mcp-playwright` |
| **cargo-llvm-cov** | Rust coverage | `cargo llvm-cov --lcov` |
| **pytest** | Python testing | `pytest --cov` |
| **vitest** | TypeScript testing | `vitest run --coverage` |

## Infrastructure Tools

| Tool | Purpose | Key Commands |
|------|---------|-------------|
| **Docker** | Containerization | `docker build`, `docker compose up` |
| **Atlas** | DB migrations | `atlas migrate apply` |
| **cbindgen** | C header generation | `cbindgen --crate my-ffi` |

## Workflow

1. `rtk init -g` for first-time team setup
1. `task setup` to install project dependencies
1. `prek install` to set up pre-commit hooks
1. `task build && task test && task lint` for development
1. `gh pr create` to submit changes
1. `rtk release` for coordinated multi-registry publishing
