______________________________________________________________________

## priority: medium

# RTK Overview

RTK (Rust Toolkit) is kreuzberg.dev's internal developer tooling CLI for managing polyglot Rust workspaces.

## Purpose

RTK provides a unified interface for common development operations across the kreuzberg.dev project ecosystem. It wraps Taskfile.yaml commands and adds workspace-aware features like cross-project version synchronization and coordinated releases.

## Key Features

- **Project scaffolding**: `rtk init` creates new projects from kreuzberg.dev templates
- **Global config**: `rtk init -g` sets up team-wide defaults and shared configuration
- **Build orchestration**: `rtk build` handles multi-crate workspace builds
- **Version sync**: `rtk sync` propagates version changes across Cargo.toml, package.json, pyproject.toml, etc.
- **Release coordination**: `rtk release` manages coordinated multi-registry publishing

## Relationship to Other Tools

- RTK wraps `task` (go-task) for build/test/lint commands
- RTK integrates with `gh` for release creation and PR workflows
- RTK uses `gcloud` for GCP deployment operations
- RTK configuration is stored in `.rtk/` directory
