______________________________________________________________________

## priority: critical

# Error Handling Strategy

**CRITICAL: OSError/RuntimeError must ALWAYS bubble up** (Python + Rust). SystemExit, KeyboardInterrupt, MemoryError too.

**Python**: Exception-based, inherit from KreuzbergError. OSError patterns: 1) Library misuse→bubble up, 2) Subprocess→analyze stderr for parsing keywords, 3) Cache→ignore, 4) Dependencies→MissingDependencyError or bubble up. Always add ~keep comments.

**Rust**: KreuzbergError::Io always bubbles up unchanged. Result\<T, KreuzbergError>, never .unwrap() in production, use `?`.

**Exception hierarchy**: ValidationError, ParsingError, OCRError, MissingDependencyError.

## Polyglot Error Handling Standardization

### Error Conversion at FFI Boundaries

- Rust `Result<T, E>` (sum type) -> Host exception/error/nil
- Use dedicated conversion functions; never expose Rust types directly
- Context must be preserved across boundary (error messages, codes)
- All error paths must be handled before returning to host language

### Language-Specific Error Patterns

- **Python**: Exception hierarchy inheriting from KreuzbergError. Custom exceptions with code, message, context. Never silent failures.
- **TypeScript**: Typed error classes inheriting from Error. Promise rejection with typed errors. Discriminated unions for catching.
- **Ruby**: Exception hierarchy from StandardError. Raise with context. Type-specific rescue. Ensure blocks for cleanup.
- **Go**: Error wrapping with `fmt.Errorf("%w", err)`. Sentinel errors with errors.Is/As. Error interface for context.
- **Java**: Checked exceptions for recoverable errors. Exception fields for details. Cause chain with initCause(). Try-with-resources.

### Error Context Preservation

- **Message**: Human-readable description
- **Error code**: Numeric (1000+) for programmatic handling
- **Source location**: File, line, column or function name
- **Context data**: Relevant variables, input preview, suggestions
- **Cause chain**: Rust anyhow, Python `from`, Go `%w`, Java `initCause`

### FFI Boundary Checklist

- All Rust errors converted to host exceptions before return
- Error messages include context (line/column, input snippet, suggestion)
- Error codes are numeric (1000+) for programmatic handling
- Cause chain preserved across languages
- Tests verify all error paths throw correct exception types
- Documentation explains each error code and recovery options

### Anti-Patterns

- Silent failures without logging or re-raising
- Losing context when converting between languages
- String-based error codes instead of numeric
- Over-wrapping errors (limit to 2-3 levels)
- Catching all errors with generic handler
- Exposing internal types at FFI boundary
