______________________________________________________________________

## priority: critical

# Workspace Dependency Management

## Cargo Workspace

Single `Cargo.toml` at root with `[workspace]` members listing all crates. `resolver = "2"` always.

## Version Synchronization

**Golden Rule**: Core library and all bindings share the same version.

- `[workspace.package]` defines `version`, `edition`, `rust-version`
- Each crate inherits: `version.workspace = true`
- Sync to non-Rust manifests via `task version:sync`

## Dependencies

- **Shared deps**: Define in `[workspace.dependencies]`, import with `{ workspace = true }`
- **Path deps**: Binding crates use `{ path = "../core", version = "X.Y.Z" }` (path for local dev, version for crates.io)
- **Explicit ranges**: `pyo3 = "0.20"` (not `"*"` or bare `"1"`)
- **Commit Cargo.lock** for reproducible builds

## MSRV

Set `rust-version` in workspace package. Test with `cargo +<msrv> test` in CI.

## Dependency Graph

Core library at root. All bindings depend on core. Never circular.

```
core-library
├── core-library-py
├── core-library-node
├── core-library-rb
├── core-library-ffi (Go, Java, C#)
├── core-library-wasm
└── core-library-cli
```

## Feature Gates

Use Cargo features for conditional compilation. Binding crates enable only needed features.

## Anti-Patterns

- Mismatched versions across crates (use workspace inheritance)
- Circular dependencies
- Uncommitted Cargo.lock
- Nested workspaces
- Wildcard dependency versions
