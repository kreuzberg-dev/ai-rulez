---
priority: critical
description: Patterns for creating and maintaining language bindings around a Rust core
---

# Polyglot Bindings

Patterns for creating and maintaining language bindings around a Rust core library.

## Binding Layer Responsibilities

- Type mapping (Rust \<-> Host language)
- Error conversion (Rust Result -> Host exceptions)
- Memory management (reference counting, GC integration)
- Concurrency adaptation (Tokio -> language runtimes)
- Language-idiomatic API surface

## Framework Quick Reference

| Language | Framework | Key Macros |
|----------|-----------|------------|
| Python | PyO3 | `#[pyclass]`, `#[pymethods]`, `#[new]` |
| TypeScript | NAPI-RS | `#[napi]`, `#[napi(constructor)]` |
| Ruby | Magnus | `#[magnus::wrap]`, `define_method` |
| PHP | ext-php-rs | `#[php_class]`, `#[php_impl]` |
| WASM | wasm-bindgen | `#[wasm_bindgen]`, `#[wasm_bindgen(constructor)]` |
| C FFI | cbindgen | `#[no_mangle] extern "C"` |

## Async Handling

- **Python**: `pyo3_asyncio::tokio::future_into_py` for async, blocking wrapper for sync
- **TypeScript**: NAPI-RS native async support
- **Ruby**: Fiber or Concurrent-ruby integration
- **Go/Java/C#**: Block on Tokio runtime via FFI layer

## Testing Requirements

- Each binding has its own language-native test suite
- Tests must cover: type conversion, error handling, async behavior, memory safety
- Coverage target: 80%+ per binding
- Integration tests verify cross-language consistency

## Anti-Patterns

- Duplicating core logic in binding code
- Exposing Rust internals to users
- Blocking in async contexts
- Missing error conversion at boundaries
- Skipping language-native tests
