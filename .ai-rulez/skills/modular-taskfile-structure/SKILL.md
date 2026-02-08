______________________________________________________________________

## priority: critical

# Modular Taskfile Structure

**Root**: Taskfile.yml (version 3) includes all modular task files from .task/ directory.

**Configuration Files**:

- `.task/config/vars.yml`: Global variables (BUILD_PROFILE, VERSION, PDFIUM_VERSION, ORT_VERSION, GOLANGCI_LINT_VERSION, paths, OS/ARCH detection, CARGO_PROFILE_DIR mapping)
- `.task/config/platforms.yml`: Platform-specific variables (EXE_EXT, LIB_EXT, NUM_CPUS with comprehensive Windows/Linux/macOS support)

**Language Files** (namespaced tasks):

- `.task/languages/rust.yml`: Rust build/test/format/lint tasks
- `.task/languages/python.yml`: Python build/test/format/lint tasks
- `.task/languages/node.yml`: TypeScript/Node.js build/test/format/lint tasks
- `.task/languages/go.yml`: Go build/test/lint tasks
- `.task/languages/java.yml`: Java build/test/lint tasks
- `.task/languages/csharp.yml`: C# build/test tasks
- `.task/languages/wasm.yml`: WebAssembly build/test tasks
- `.task/languages/ruby.yml`: Ruby build/test/lint tasks
- `.task/languages/php.yml`: PHP build/test/lint tasks

**Workflow Orchestration Files** (internal, cross-language):

- `.task/workflows/build.yml`: build, build:all, build:all:dev, build:all:release
- `.task/workflows/test.yml`: test, test:all, test:all:fast
- `.task/workflows/lint.yml`: lint, lint:all, lint:check
- `.task/workflows/e2e.yml`: e2e, e2e:all, e2e:fast

**Tool Task Files**:

- `.task/tools/general.yml`: setup, clean, setup-pre-commit, pre-commit
- `.task/tools/version-sync.yml`: version:sync (sync version across all manifests)
- `.task/tools/pdfium.yml`: pdfium:install, pdfium:setup
- `.task/tools/pre-commit.yml`: pre-commit configuration
- `.task/tools/docs.yml`: Documentation generation tasks
- `.task/tools/smoke.yml`: Smoke tests
- `.task/test-config.yml`: Test configuration

**Namespace Convention**:

- Language tasks: `task rust:build`, `task python:test`, `task node:lint`
- Workflow tasks: `task build:all`, `task test:all`, `task lint:check`
- Tool tasks: `task version:sync`, `task pdfium:install`, `task setup`, `task clean`

## Taskfile Best Practices & Guidelines

**Modular Design Principles**:

- Each language gets its own task file in `.task/languages/`
- Workflows orchestrated in `.task/workflows/`
- Configuration in `.task/config/` (vars.yml, platforms.yml)
- Tool tasks in `.task/tools/`
- Main Taskfile.yml is minimal - just includes and top-level entry points

**Creating New Task Files**:

1. Create `.task/languages/{language}.yml` for language-specific tasks
1. Include in main Taskfile.yml
1. Use namespace pattern: `task {language}:build`, `task {language}:test`
1. Support BUILD_PROFILE for dev/release/ci variants

**Variable Management**:

- Global variables in `.task/config/vars.yml` (BUILD_PROFILE, VERSION, paths, OS/ARCH)
- Platform-specific in `.task/config/platforms.yml` (EXE_EXT, LIB_EXT, NUM_CPUS)
- Avoid hardcoding paths; use {{.ROOT}}, {{.CRATES_DIR}}, {{.PACKAGES_DIR}}, {{.TARGET_DIR}}

**Task Naming Convention**:

- Language tasks: `task {language}:{action}` (e.g., rust:build, python:test)
- Workflow tasks: `task {workflow}:{scope}` (e.g., build:all, test:all:fast)
- Tool tasks: `task {tool}:{action}` (e.g., version:sync, pdfium:install)
- Variants: `:dev`, `:release`, `:ci`, `:fast`, `:check`

**Cross-Platform**: Test on Windows, Linux, macOS. Use ENV variables for library paths.

**Error Handling**: Use `ignore_error: true` sparingly; prefer explicit error handling.

**Caching & Performance**: Leverage {{.NUM_CPUS}} for parallel builds. Use `:fast` variants for quick validation.

## Build Automation Standards

- Lock files committed: uv.lock, pnpm-lock.yaml, go.sum, Cargo.lock, composer.lock
- Dependency minimization: justify all external deps, audit regularly
- Version sync across runtimes: Cargo.toml, package.json, pyproject.toml
