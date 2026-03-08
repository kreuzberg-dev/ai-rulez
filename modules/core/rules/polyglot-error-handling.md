______________________________________________________________________

## priority: high

# Polyglot Error Handling

Cross-language error handling standards for Rust-core polyglot projects.

## Error Conversion at FFI Boundaries

- Rust `Result<T, E>` (sum type) -> Host exception/error/nil
- Use dedicated conversion functions; never expose Rust types directly
- Context must be preserved across boundary (error messages, codes)
- All error paths must be handled before returning to host language

## Language-Specific Error Patterns

- **Rust**: `Result<T, E>` with thiserror; never `.unwrap()` in production; use `?` operator
- **Python**: Exception hierarchy inheriting from base error. Custom exceptions with code, message, context
- **TypeScript**: Typed error classes inheriting from Error. Promise rejection with typed errors
- **Ruby**: Exception hierarchy from StandardError. Raise with context. Type-specific rescue
- **Go**: Error wrapping with `fmt.Errorf("%w", err)`. Sentinel errors with `errors.Is/As`
- **Java**: Checked exceptions for recoverable errors. Cause chain with `initCause()`
- **C#**: Exception hierarchy with InnerException chain

## Error Context Preservation

- **Message**: Human-readable description
- **Error code**: Numeric (1000+) for programmatic handling
- **Source location**: File, line, column or function name
- **Context data**: Relevant variables, input preview, suggestions
- **Cause chain**: Rust anyhow, Python `from`, Go `%w`, Java `initCause`

## FFI Boundary Checklist

- All Rust errors converted to host exceptions before return
- Error messages include context (line/column, input snippet, suggestion)
- Error codes are numeric (1000+) for programmatic handling
- Cause chain preserved across languages
- Tests verify all error paths throw correct exception types

## Anti-Patterns

- Silent failures without logging or re-raising
- Losing context when converting between languages
- String-based error codes instead of numeric
- Over-wrapping errors (limit to 2-3 levels)
- Catching all errors with generic handler
- Exposing internal Rust types at FFI boundary
