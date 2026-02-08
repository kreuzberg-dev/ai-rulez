______________________________________________________________________

## priority: critical

# FFI and Language Interop Standards

FFI code is a **contract** between languages. Breaking it causes crashes.

## Core Principles

1. **Explicit ownership**: Every pointer has one owner. Document with SAFETY comments.
1. **Opaque handles**: Never expose Rust types directly. Use `#[repr(transparent)]` handle wrappers.
1. **Null safety**: Check ALL pointers before use. Return null on error.
1. **No use-after-free/double-free**: Document pointer invalidation in doc comments.

## Patterns

- **Allocate/free pairs**: Every `_new()` has a matching `_free()`. Caller owns returned `*mut`.
- **Borrowed returns**: `*const` means Rust owns it; valid until next call or handle free.
- **Static returns**: `b"1.0.0\0".as_ptr()` — document "must NOT free".

## cbindgen

Generate C headers automatically from Rust `#[no_mangle] extern "C"` functions:

```toml
# cbindgen.toml
language = "C"
include_guard = "MY_LIB_FFI_H"
[export]
exclude = ["internal_*"]
```

```bash
cbindgen --crate my-lib-ffi -o include/my_lib.h
cbindgen --verify --crate my-lib-ffi  # CI check
```

## SAFETY Comments

Every `unsafe` block needs a SAFETY comment: what invariant, why it holds, what breaks if violated.

```rust
// SAFETY: `ptr` is non-NULL (checked above), allocated by _new(), not yet freed
unsafe { Box::from_raw(ptr) }
```

## Language Integration

| Language | FFI Mechanism | Key Pattern |
|----------|--------------|-------------|
| Go | cgo `#include "lib.h"` | `C.CString` → defer `C.free`; `C.GoString` for returns |
| Java | Panama FFM (21+) | `Linker.downcallHandle` + `FunctionDescriptor` |
| C# | P/Invoke | `[DllImport]` with `CallingConvention.Cdecl` |

## Anti-Patterns

- Exposing Rust types directly across FFI (use opaque handles)
- Missing null checks (segfault on bad input)
- Returning stack-allocated data (dangling pointer)
- No SAFETY comments on unsafe blocks
- Forgetting free function for allocated resources
