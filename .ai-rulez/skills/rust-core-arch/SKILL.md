______________________________________________________________________

## priority: medium

# Rust Core Architecture

## Core Library Crate

- **Library crate**: crates/core-library/ implements the core domain logic
- **Domain focus**: Clear separation of concerns with focused business logic
- **Error handling**: thiserror for ergonomic custom errors
- **Processing pipeline**: Parse input → Transform data → Validate result → Format output
- **Performance**: zero-copy where possible, streaming for large data structures
- **Dependencies**: Leverage the Rust ecosystem (serde, tracing, thiserror, etc.)

## Integration Points

- **PyO3 bindings**: crates/core-library-py exports Rust API to Python
- **NAPI-RS bindings**: crates/core-library-node for Node.js/Bun
- **Magnus bindings**: Ruby gem uses Magnus for clean FFI
- **ext-php-rs bindings**: crates/core-library-php for PHP extension
- **WASM**: crates/core-library-wasm for browser/Wasmtime
- **FFI library**: crates/core-library-ffi for C-compatible exports (Go, Java, C#)

## Testing Strategy

- Doc tests on public types with realistic domain examples
- Unit tests per module (core components, utilities, validators)
- Integration tests with actual data fixtures in crates/core-library/tests/
- Benchmarks in benches/ with criterium for performance-critical paths
- Coverage: cargo-llvm-cov with 95% threshold

## Rust Edition & Quality Standards

**Rust 2024 edition - High strictness - clippy -D warnings - 95% coverage - Zero unwrap**

- Rust edition 2024; cargo fmt, clippy with -D warnings (zero tolerance)
- Result\<T, E> for errors; thiserror for custom errors; NEVER .unwrap() in production
- Documentation: rustdoc on ALL public items with examples, SAFETY comments for unsafe
- Async: Tokio 1.x exclusively, 'static constraints, proper Send+Sync bounds
- FFI: isolated modules, pointer validation, SAFETY comments, error conversion at boundaries
- Code quality: RAII principle, explicit lifetimes, builder pattern, no panics
- Pre-commit: cargo fmt, clippy, test, tarpaulin coverage check
- Never: unwrap in production, unsafe without SAFETY docs, std::thread (use Tokio)

## Rust Polyglot Conventions

**Naming**: PascalCase (types), snake_case (fns/vars/modules), SCREAMING_SNAKE_CASE (consts).

**Error handling**: Result\<T, Error>, never .unwrap() in production, use `?`, IO errors bubble up properly, SAFETY comments for unsafe code, handle lock poisoning.

**Async**: Tokio throughout, #[tokio::main]/#[tokio::test], provide \_sync wrappers, never std::thread::sleep.

**Memory**: Arc for shared ownership, Mutex/RwLock for interior mutability, streaming for large data, RAII patterns.

**Performance**: ahash for HashMap, lazy_static/once_cell, SIMD where appropriate, zero-copy (&str/&[u8]).

**Plugins**: Traits for extensibility, Arc<dyn Trait> storage, Send+Sync requirements, registry pattern.

**Zero clippy warnings** (cargo clippy -- -D warnings).

**Core structure**: Modular organization with trait-based extensibility. Plugin flow: Input->Registry->Handler->Pipeline->Output.

## Module Organization & Public API Design

**Modules are organizational units, not visibility boundaries**. Use `pub` / `pub(crate)` / `pub(super)` to control visibility explicitly.

1. **Root crate module**: `src/lib.rs` or `src/main.rs` re-exports public items
1. **Feature-specific modules**: `src/parsing/`, `src/conversion/`, `src/utils/` organize by domain
1. **Internal vs. public**: Mark private modules with `pub(crate)` if they're internal infrastructure
1. **Re-exports**: Use `pub use` in `lib.rs` to define the public API surface

**Public API Design Golden Rule**: Your `src/lib.rs` should read like a user guide.

**Visibility**:

- **Public** (`pub`): Part of stable API contract (main types, error types, config builders)
- **Pub(crate)** (`pub(crate)`): Internal infrastructure, not for external users
- **Private** (no visibility keyword): Never exposed

**Feature Gates**: Use Cargo features to conditionally expose API surface.

**Module Re-exports**: Module `mod.rs` re-exports public items; root `lib.rs` flattens for users.

**Cargo Public API Validation**: Use `cargo-public-api` to track breaking changes in CI.

**Anti-patterns**: Exposing implementation details, deeply nested modules in public API, mixed public/private in single module, not documenting API stability.

**Stability Markers**: Document API stability with `#[deprecated]` and doc comments.
