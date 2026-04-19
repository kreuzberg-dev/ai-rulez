---
priority: critical
---

- Bindings are minimal glue: call Rust core, convert types, convert errors — no business logic
- Crate naming: {lib}-py (PyO3), {lib}-node (NAPI-RS), {lib}-rb (Magnus), {lib}-php (ext-php-rs), {lib}-wasm (wasm-bindgen), {lib}-ffi (C FFI)
- Distribution: packages/python/ (PyPI), typescript/ (npm), ruby/ (RubyGems), php/ (Composer), go/ (Go module), java/ (Maven), csharp/ (NuGet)
- Each binding has its own language-native test suite — 80%+ coverage
- Error conversion must preserve context (message + numeric code) at every FFI boundary
- Async: pyo3_asyncio (Python), native #[napi] async (TS), Fiber (Ruby), block on Tokio via FFI (Go/Java/C#)
