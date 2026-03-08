______________________________________________________________________

## priority: high summary: Fixture-driven cross-language e2e test generation architecture

# E2E Generator Architecture

## Overview

The e2e generator is a Rust CLI tool that reads JSON fixture definitions and produces complete, runnable test suites for each supported language binding. This ensures test parity across all language ecosystems from a single source of truth.

## Data Flow

```
fixtures/*.json → Generator CLI (Rust, clap) → e2e/{language}/ output
```

1. **Load**: Recursively scan `fixtures/` for JSON files, deserialize with serde.
1. **Validate**: Check for duplicate IDs, missing required fields, invalid categories.
1. **Sort**: Group fixtures by (category, id) for deterministic output.
1. **Generate**: For each target language, invoke the `Generator` trait implementation.
1. **Write**: Emit complete test project files to `e2e/{language}/`.

## Generator Trait

```rust
pub trait Generator {
    fn generate(&self, fixtures: &[Fixture], output_dir: &Path) -> Result<(), String>;
    fn name(&self) -> &'static str;
}
```

Each language implements this trait, handling:

- String escaping (raw strings, heredocs, triple-quotes)
- Test framework boilerplate (imports, setup, teardown)
- Assertion translation (fixture assertions → language-native asserts)
- Project scaffold (dependency files, build configs)

## Language Support by Repo

| Repo | Rust | Python | TypeScript | Ruby | Go | Java | Elixir | C |
|------|------|--------|------------|------|----|------|--------|---|
| spikard | Y | Y | Y | Y | - | - | Y | - |
| kreuzberg | Y | Y | Y | Y | - | - | - | - |
| tree-sitter-language-pack | Y | Y | Y | - | Y | Y | Y | Y |

## Taskfile Integration

```yaml
generate:e2e:
  desc: Regenerate e2e test suites from fixtures
  cmds:
    - cargo run -p e2e-generator -- generate --output-dir e2e/

test:e2e:
  desc: Run all e2e test suites
  deps: [generate:e2e]
  cmds:
    - task test:e2e:rust
    - task test:e2e:python
    - task test:e2e:typescript
    # ... per language
```
