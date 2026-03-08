______________________________________________________________________

## priority: high

# E2E Generator Conventions

Fixture-driven test generation across language bindings.

## Pattern

- A Rust CLI tool under `tools/e2e-generator/` (or `tools/test-generator/`) reads JSON fixtures and generates complete, runnable test suites for each supported language binding.
- The generator implements a `Generator` trait with `generate(&self, fixtures: &[Fixture], output_dir: &Path) -> Result<(), String>` and `name() -> &'static str` per target language.
- Output goes to `e2e/{language}/` directories, each containing a fully configured test project (Cargo.toml, pyproject.toml, package.json, Gemfile, etc.).

## Fixture Organization

- Fixtures are JSON files under `tools/e2e-generator/fixtures/` (or `tools/test-generator/fixtures/`), organized by category subdirectories.
- Categories: `smoke`, `basic`, `parsing`, `edge-case`, `error-handling`, `platform-specific`.
- Fixtures are loaded recursively, sorted by (category, id), with duplicate ID detection at load time.
- Each fixture has a unique snake_case `id` field.

## Generation Workflow

- Run `task generate:e2e` to regenerate all test suites from fixtures.
- The generator produces language-specific string escaping, test framework boilerplate, and assertion helpers.
- Generated test projects must be self-contained and immediately runnable with the language's standard test command.

## Never

- Hand-edit generated test files; modify fixtures or the generator source instead.
- Duplicate test logic across languages; fixtures are the single source of truth.
- Skip platform-specific skip conditions in fixtures.
- Add tests directly to `e2e/` directories; all tests come from the generator.
