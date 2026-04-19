______________________________________________________________________

## priority: critical

- No Any types (Python, TypeScript) — use Unknown/generics
- No class-based tests (Python) — function-based only
- No mocking internal services — use real objects
- No manual dependency management — use lock files
- No blocking I/O in async — fully async paths
- No bare exception handlers — catch specific types
- No magic numbers — named constants
- No inheritance for code reuse — composition
- No global state — dependency injection
- No f-strings in logging — structured key=value
- No core logic duplication in bindings — Rust only
