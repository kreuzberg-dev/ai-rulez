---
priority: critical
---

- Every pointer has one owner, documented with SAFETY comments
- Opaque handles only — never expose Rust types directly, use #[repr(transparent)] wrappers
- Null safety: check ALL pointers before use, return null + error code on failure
- Allocate/free pairs: every \_new() has a matching \_free(), caller owns \*mut
- Every unsafe block has a SAFETY comment: what invariant, why it holds, what breaks if violated
- Generate C headers with cbindgen — CI verifies generated headers match committed
- Semantic versioning for C headers, struct layouts frozen at MAJOR.MINOR boundaries
- All exported functions use #[no_mangle] extern "C"
- Rust Result\<T, E> → host exceptions via dedicated conversion functions at every boundary
- Preserve error context across boundaries: message, numeric code (1000+), source location, cause chain
