______________________________________________________________________

## priority: medium

# RTK Usage

Standards for using RTK (Rust Toolkit) in kreuzberg.dev projects.

## Setup

- Initialize with `rtk init -g` for global configuration
- Use `rtk init` in project root for project-specific setup
- RTK configuration lives in `.rtk/` directory

## Command Groups

- `rtk build` - Build orchestration across workspace members
- `rtk test` - Test runner with coverage integration
- `rtk lint` - Unified linting across Rust and binding crates
- `rtk release` - Coordinated release across all packages
- `rtk sync` - Version synchronization across manifests

## Team Standards

- All team members use RTK for local development workflow
- CI pipelines invoke RTK commands for consistency with local dev
- RTK configuration is committed to version control
- Custom RTK plugins stored in `.rtk/plugins/`
- Use `rtk doctor` to verify environment setup

## Integration

- RTK integrates with Taskfile.yaml as the underlying task runner
- RTK wraps common multi-step workflows into single commands
- Use RTK for new project scaffolding with kreuzberg.dev templates

## Anti-Patterns

- Bypassing RTK with direct cargo/npm/pip commands in CI
- Not committing RTK configuration
- Using RTK init without -g flag for shared team setup
