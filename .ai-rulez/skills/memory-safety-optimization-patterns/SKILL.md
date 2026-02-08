______________________________________________________________________

## priority: high

# Memory Safety & Optimization Patterns

## Zero-Copy

- `&str` over `String`, `&[T]` over `Vec<T>` in function params
- `Cow<T>` for conditional ownership (borrow when possible, clone only on mutation)
- `Arc<T>` for shared immutable data across threads

## RAII

Resources released automatically on scope exit via `Drop`. Mutex/RwLock guards auto-unlock. No manual cleanup needed.

## String Handling

- `Cow<str>` for conditional String/&str ownership
- `Arc<str>` for cheap cloning of shared immutable strings
- `String::with_capacity()` to pre-allocate

## Buffer Reuse

- `Vec::clear()` + reuse instead of new allocation
- Pre-allocate with `Vec::with_capacity()` in hot loops
- `SmallVec` for stack-allocated small collections

## Lifetimes

- Default to references in function params
- Minimize lifetimes in structs (prefer owned data)
- Use `'_` wildcard for unused lifetime bounds

## Verification Tools

| Tool | Purpose | Command |
|------|---------|---------|
| Valgrind | Memory leaks, UAF | `valgrind --leak-check=full ./app` |
| ASan | Memory errors | `RUSTFLAGS="-Z sanitizer=address" cargo +nightly build` |
| Miri | Undefined behavior | `cargo +nightly miri test` |
| Clippy | Lifetime/safety lints | `cargo clippy -- -D warnings` |

## Anti-Patterns

- `Clone` instead of `&T` (unnecessary allocation)
- `format!()` when borrowing suffices
- Holding mutex guards across `.await` points
- `Box<T>` for simple owned data (unnecessary indirection)
- No buffer pre-allocation in hot paths
