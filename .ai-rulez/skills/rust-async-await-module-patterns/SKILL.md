______________________________________________________________________

## priority: medium

# Rust Async/Await & Module Patterns

## Tokio Runtime

**Single runtime instance** shared across the application via `OnceLock<Runtime>`.

- **Library bindings**: `block_on()` wrapper using global `OnceLock<Runtime>` for sync FFI
- **NAPI-RS**: Thread-local `AsyncWorkerPool` with `runtime.spawn()`
- **Feature-gated**: `#[cfg(feature = "async-runtime")]` for conditional async paths

## Send + Sync in FFI

ALL types exposed to FFI must be `Send + Sync`. Compile-time verification:

```rust
fn _assert_send_sync() {
    fn check<T: Send + Sync>() {}
    check::<MyType>();
}
```

| Type | Send+Sync | Fix |
|------|-----------|-----|
| `Rc<T>` | No | `Arc<T>` |
| `RefCell<T>` | No | `Mutex<T>` |
| `Cell<T>` | No | `AtomicU32` etc. |
| `*const T` | No | `Box<T>` or safe wrapper |

## Blocking Bridge (sync languages)

For Python/Ruby/PHP: `block_on(spawn_blocking(|| ...))` — prevents blocking Tokio workers.

For long-running ops: channel-based `AsyncBridge` with `mpsc::UnboundedSender<Task>` + callback pattern.

## Module Organization

```
src/
├── lib.rs          # Main API (sync re-exports)
├── async/          # AsyncConverter, streaming, runtime
├── sync/           # Converter (sync)
└── bridge.rs       # sync/async bridge layer
```

Feature gate: `#[cfg(feature = "async-runtime")] pub use async_::AsyncConverter;`

## Testing

- `#[tokio::test]` for unit tests
- `#[tokio::test(flavor = "multi_thread")]` for concurrency tests
- Integration tests call sync wrappers directly

## Anti-Patterns

- Creating new runtime per call (reuse single instance)
- `std::thread::sleep` in async context (use `spawn_blocking`)
- `RefCell` in FFI types (use `Mutex`)
- Panics in async contexts (kills runtime; return `Result`)
- Holding mutex guards across `.await` points
