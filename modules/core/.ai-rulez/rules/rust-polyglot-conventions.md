______________________________________________________________________

## priority: high

- Rust 2024 edition, clippy -D warnings, cargo fmt
- Result\<T, E> with thiserror — never .unwrap() or panic in library code
- 95% test coverage (cargo-llvm-cov), unit + integration + doc tests
- rustdoc on all public items, SAFETY comments on all unsafe blocks
- Tokio 1.x exclusively, 'static + Send + Sync bounds on public futures
- All public types FFI-friendly or have FFI equivalents (#[repr(C)])
- Avoid Rust-specific idioms that can't be expressed in target languages
- Cargo.toml version is single source of truth — synced to all binding manifests
