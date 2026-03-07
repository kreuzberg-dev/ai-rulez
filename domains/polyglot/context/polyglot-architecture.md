______________________________________________________________________

## priority: high

# Polyglot Architecture Context

Kreuzberg is built on a Rust-first architecture: all extraction logic, transformation algorithms, and complex computations live in the Rust core library. Language bindings are thin wrappers exposing language-idiomatic APIs.

## Architecture Pattern

```
Consuming Applications (Python, TypeScript, Ruby, Java, PHP, Go, etc.)
    |
Language-Specific Bindings Layer (PyO3, NAPI-RS, Magnus, ext-php-rs, JNI/FFI, cgo)
    |
FFI/Interop Layer (C-compatible interfaces for some languages)
    |
Rust Core Library (crates/kreuzberg)
```

## Supported Language Ecosystems

| Language | Framework | Package Manager | Test Framework |
|----------|-----------|-----------------|----------------|
| Python | PyO3 | PyPI | pytest |
| TypeScript | NAPI-RS | npm | vitest |
| Ruby | Magnus | RubyGems | RSpec |
| PHP | ext-php-rs | Composer | PHPUnit |
| Java | JNI/FFI | Maven | JUnit |
| Go | cgo | Go modules | Go testing |
| C# | P/Invoke | NuGet | xUnit |
| Elixir | Rustler NIF | Hex | ExUnit |
| WebAssembly | wasm-bindgen | npm | JavaScript |

## Key Principles

1. Rust core is single source of truth for all business logic
1. Bindings are thin wrappers -- no duplicate logic
1. All bindings expose equivalent APIs (feature parity)
1. Version in Cargo.toml drives all binding package versions
1. 95% coverage for Rust core, 80%+ for bindings
