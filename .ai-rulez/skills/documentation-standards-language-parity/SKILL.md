______________________________________________________________________

## priority: critical

# Documentation Standards & Language Parity

**CRITICAL: Full language parity required**. ALL documentation, guides, and code examples MUST include snippets for ALL 7 supported languages: Rust, Python, TypeScript, Ruby, Java, Go, C#.

**Code snippet structure**:

- Location: docs/snippets/{language}/{category}/{filename}.{ext}
- Categories: getting-started, config, advanced, ocr, metadata, plugins, api, mcp, cli, utils, cache, docker, benchmarking
- Use MkDocs pymdownx.tabbed extension for multi-language code blocks
- Include snippets with --8\<-- syntax: `--8<-- "docs/snippets/python/config/basic.py"`

**Snippet requirements**:

- Keep concise (10-40 lines per snippet)
- No comments in snippets (documentation provides context)
- Verify API accuracy by reading source code before writing
- Test all snippets for correctness
- Each language must have equivalent functionality shown

**Language-specific inline documentation**:

- **Rust**: /// doc comments on ALL public items, SAFETY comments for unsafe, ~keep suffix for error handling, examples as doctests
- **Python**: NO docstrings in private/test files, public API only, Google style format
- **TypeScript**: JSDoc with @param/@returns/@example on ALL exports
- **Java**: Javadoc on ALL public classes/methods with @param/@return/@throws/@since
- **Go**: Package doc.go files, inline comments following Go conventions
- **Ruby**: YARD documentation with @param/@return tags
- **C#**: XML doc comments with <summary>, <param>, <returns>

**General rules**:

- Code comments explain "why" not "what"
- No proactive README/documentation creation - only when requested
- No AI signatures in any documentation
- Cross-reference related APIs across languages
- Update ALL language snippets when APIs change

## Polyglot API Documentation Examples

**CRITICAL: Complete language parity for API documentation.** ALL public APIs MUST be documented with examples in all supported languages.

### Documentation Tools by Language

| Language | Tool | Key Strengths |
|----------|------|---------------|
| **Rust** | rustdoc | Markdown, examples as doctests, cross-referencing |
| **Python** | Sphinx | reStructuredText, autodoc, napoleon for Google style |
| **TypeScript** | TypeDoc | JSDoc parsing, template customization, JSON exports |
| **Ruby** | YARD | @param/@return/@example, @overload support |
| **PHP** | PHPDocumentor | @param/@return/@throws, markdown descriptions |
| **Java** | Javadoc | @param/@return/@throws/@since/@deprecated tags |
| **Go** | godoc | doc.go files, package-level docs, examples in \_test.go |
| **C#** | DocFX | XML doc comments, xref links, TOC generation |
| **Elixir** | ExDoc | Markdown, @doc/@spec, @deprecated, live examples |
| **WebAssembly** | wasm-doc / JSDoc | JSDoc for JS wrapper, inline comments |

### API Documentation Structure

Every public API must include:

1. **Summary**: One-line description
1. **Description**: Detailed behavior, edge cases, preconditions
1. **Parameters**: Type, description, constraints, defaults
1. **Return Value**: Type, description, error states
1. **Examples**: Minimal working code in ALL supported languages
1. **Related APIs**: Cross-references
1. **Error Cases**: Common errors and handling
1. **Performance Notes**: Complexity, memory usage

### Migration Guide

1. Document Rust core first (canonical implementation)
1. Update all language bindings with equivalent documentation
1. Add examples in docs/snippets/{language}/{category}/
1. Build and verify documentation for each language

### Documentation Build Verification

```bash
task doc:rust      # cargo doc
task doc:python    # sphinx-build
task doc:typescript # typedoc
task doc:ruby      # yard
task doc:php       # phpdoc
task doc:java      # javadoc
task doc:go        # go doc
task doc:csharp    # docfx
task doc:elixir    # mix docs
task doc:wasm      # jsdoc
```

### Error Handling Documentation

ALL error documentation must use language-native patterns (Rust `# Errors`, Python `Raises:`, TypeScript `@throws`, Ruby `@raise`, PHP `@throws`, Java `@throws`, Go returns error, C# `<exception>`, Elixir docstring).

### Best Practices

1. Keep examples concise (5-15 lines max)
1. Use realistic scenarios matching actual usage
1. Include error handling in each language
1. Test all code examples as part of CI/CD
1. Update ALL languages together when APIs change
1. Automate generation to reduce manual burden
