______________________________________________________________________

## priority: high

# Fixture-Driven Testing Strategy

## Shared Fixtures Across Bindings

- **Single source of truth**: Fixtures defined once in Rust (`crates/core/fixtures/`)
- **Symlinked** into each binding's test directory
- **Format**: JSON with `version`, `generated_at`, `test_cases[]` (each has `name`, `input`, `expected_output` or `expected_error`)
- **Generation**: Rust `#[test] #[ignore]` generates fixtures from canonical implementation

## Fixture Consumption

Each language loads the same JSON and parameterizes tests:

| Language | Test Framework | Fixture Loading |
|----------|---------------|----------------|
| Rust | `#[test]` | `serde_json::from_str` |
| Python | `pytest.mark.parametrize` | `json.load()` from fixture path |
| TypeScript | `describe/forEach` | `JSON.parse(fs.readFileSync(...))` |
| Ruby | `RSpec each` | `JSON.parse(File.read(...))` |
| Java | `@MethodSource` | `Resources.toString(getResource(...))` |
| Go | `t.Run` table-driven | `os.ReadFile` + `json.Unmarshal` |

## Snapshot Testing

Use `insta` (Rust), `pytest-snapshot` (Python), Jest snapshots (TS) for complex outputs.

## Cross-Language Parity

Run `scripts/test_parity.sh` or CI job to validate identical behavior across all bindings.

## Testing Standards

- **Three-tier**: unit (80-95%), integration (real services), E2E (smoke/full)
- **Rust**: `cargo test`, `#[tokio::test]`, 95% coverage, test error paths
- **Python**: Function-based tests, pytest, 95% coverage. Naming: `test_<function>_<scenario>_<outcome>`
- **TypeScript**: `.spec.ts` next to source, vitest, 80%+
- **Go**: `*_test.go`, table-driven `t.Run()`, `go test -race`, 80%+
- **Ruby**: RSpec `describe/context/it`, 80%+
- **Java**: JUnit 5, AssertJ, 80%+

## Test Apps (Published Package Validation)

`tests/test_apps/` validates PUBLISHED packages (npm, PyPI, RubyGems, etc.) — NOT for local development. Included in `task sync-versions`.

## Anti-Patterns

- Language-specific fixtures (define once, reuse everywhere)
- Hardcoded test data (use fixture files)
- No error case testing
- Manual fixture updates (generate from canonical impl)
- Mocking anyio/asyncio (prefer real objects)
