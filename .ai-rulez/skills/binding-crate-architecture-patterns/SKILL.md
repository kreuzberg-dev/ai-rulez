______________________________________________________________________

## priority: high

# Binding Crate Architecture Patterns

## Principles

1. **Minimal wrapper**: Bindings are glue only — call Rust core, convert types, convert errors
1. **Type translation**: Clear Rust ↔ host language mapping
1. **Error conversion**: Rust errors → native exceptions/error types
1. **Memory safety**: Respect language-specific ownership models
1. **Testing**: Language-native test suite, not just Rust tests

## Crate Naming

`{lib}-{language}`: `-py` (PyO3), `-node` (NAPI-RS), `-rb` (Magnus), `-php` (ext-php-rs), `-wasm` (wasm-bindgen), `-ffi` (C FFI for Go/Java/C#)

## Framework Patterns

| Framework | Macro | Constructor | Error Pattern |
|-----------|-------|-------------|--------------|
| PyO3 | `#[pyclass]` / `#[pymethods]` | `#[new]` | `.map_err(\|e\| PyException::new_err(e.to_string()))` |
| NAPI-RS | `#[napi]` | `#[napi(constructor)]` | `napi::Error::new(Status::GenericFailure, msg)` |
| Magnus | `#[magnus::wrap]` | `define_method("new", ...)` | `Into<magnus::RError>` |
| ext-php-rs | `#[php_class]` / `#[php_impl]` | `pub fn new()` | Return error string |
| wasm-bindgen | `#[wasm_bindgen]` | `#[wasm_bindgen(constructor)]` | `JsValue::from_str(&e.to_string())` |
| C FFI | `#[no_mangle] extern "C"` | `_new() -> Handle` | Return null + error code |

## Type Mapping

| Rust | Python | Node.js | Ruby | C/FFI |
|------|--------|---------|------|-------|
| `String`/`&str` | `str` | `string` | `String` | `char*`/`const char*` |
| `u64` | `int` | `BigInt` | `Integer` | `uint64_t` |
| `Result<T>` | Exception | Error thrown | raises | null/error code |
| `Option<T>` | `None/T` | `null/T` | `nil/T` | `NULL/T` |

## Distribution (packages/)

python/ (PyPI), typescript/ (npm), ruby/ (RubyGems), php/ (Composer), go/ (Go module), java/ (Maven), csharp/ (.NET NuGet)

## Anti-Patterns

- Exposing Rust internals to users
- Blocking in async contexts (use `spawn_blocking`)
- Dangling pointers in FFI returns
- Panicking instead of returning errors (`unwrap()` in binding code)
