______________________________________________________________________

## priority: high

# Rust Polyglot Conventions

Rust conventions for polyglot projects where Rust serves as the core library with multi-language bindings.

## Core Standards

- Rust 2024 edition; cargo fmt, clippy with -D warnings (zero tolerance)
- Result\<T, E> for errors; thiserror for custom errors; NEVER .unwrap() in production
- Testing: 95% minimum coverage (cargo-llvm-cov), unit/integration/doc tests in crates/
- Documentation: rustdoc on ALL public items with examples, SAFETY comments for unsafe
- Async: Tokio 1.x exclusively, 'static constraints, proper Send+Sync bounds

## Polyglot-Specific Rust Guidelines

- Core libraries define the canonical API surface; bindings are thin wrappers
- All public types must be FFI-friendly or have FFI-compatible equivalents
- Use `#[repr(C)]` for types crossing FFI boundaries
- Avoid Rust-specific idioms that cannot be expressed in target languages
- Design error types with cross-language conversion in mind (error codes + messages)
- Version in Cargo.toml is the single source of truth for all binding packages

## Module Organization

- `crates/{lib}/` - core Rust library
- `crates/{lib}-py/` - PyO3 bindings
- `crates/{lib}-node/` - NAPI-RS bindings
- `crates/{lib}-ffi/` - C FFI layer for Go/Java/C#
- `crates/{lib}-wasm/` - wasm-bindgen bindings
- `packages/` - language-specific package wrappers

## Anti-Patterns

- Unwrap in production code
- Unsafe without SAFETY documentation
- Panics in library code
- Exposing Rust-specific types (Vec, HashMap) directly across FFI
- Duplicating business logic in binding crates
