______________________________________________________________________

## priority: critical

# Modular Taskfile Structure

**Root**: Taskfile.yml (version 3) includes all modular task files from .task/ directory.

## Configuration Files

- `.task/config/vars.yml`: Global variables (BUILD_PROFILE, VERSION, paths, OS/ARCH detection)
- `.task/config/platforms.yml`: Platform-specific variables (EXE_EXT, LIB_EXT, NUM_CPUS)

## Language Files (namespaced tasks)

- `.task/languages/rust.yml`, `python.yml`, `node.yml`, `go.yml`, `java.yml`, `csharp.yml`, `wasm.yml`, `ruby.yml`, `php.yml`

## Workflow Orchestration Files

- `.task/workflows/build.yml`, `test.yml`, `lint.yml`, `e2e.yml`

## Tool Task Files

- `.task/tools/general.yml`, `version-sync.yml`, `pdfium.yml`, `pre-commit.yml`, `docs.yml`, `smoke.yml`

## Namespace Convention

- Language tasks: `task rust:build`, `task python:test`, `task node:lint`
- Workflow tasks: `task build:all`, `task test:all`, `task lint:check`
- Tool tasks: `task version:sync`, `task pdfium:install`, `task setup`, `task clean`

## Best Practices

- Each language gets its own task file in `.task/languages/`
- Main Taskfile.yml is minimal -- just includes and top-level entry points
- Avoid hardcoding paths; use variables
- Support BUILD_PROFILE for dev/release/ci variants
- Lock files committed: uv.lock, pnpm-lock.yaml, go.sum, Cargo.lock, composer.lock
