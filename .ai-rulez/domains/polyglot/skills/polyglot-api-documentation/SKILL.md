______________________________________________________________________

## priority: critical

# Polyglot API Documentation

Full language parity required for all documentation, guides, and code examples across all supported languages.

## Documentation Tools by Language

| Language | Tool | Key Strengths |
|----------|------|---------------|
| Rust | rustdoc | Markdown, examples as doctests, cross-referencing |
| Python | Sphinx | reStructuredText, autodoc, napoleon for Google style |
| TypeScript | TypeDoc | JSDoc parsing, template customization |
| Ruby | YARD | @param/@return/@example, @overload support |
| PHP | PHPDocumentor | @param/@return/@throws, markdown descriptions |
| Java | Javadoc | @param/@return/@throws/@since/@deprecated tags |
| Go | godoc | doc.go files, package-level docs |
| C# | DocFX | XML doc comments, xref links |
| Elixir | ExDoc | Markdown, @doc/@spec, @deprecated |

## API Documentation Structure

Every public API must include:

1. **Summary**: One-line description
1. **Parameters**: Type, description, constraints, defaults
1. **Return Value**: Type, description, error states
1. **Examples**: Minimal working code in ALL supported languages
1. **Error Cases**: Common errors and handling

## Code Snippet Structure

- Location: `docs/snippets/{language}/{category}/{filename}.{ext}`
- Keep concise (10-40 lines per snippet)
- No comments in snippets (documentation provides context)
- Verify API accuracy by reading source code before writing
- Each language must have equivalent functionality shown

## Language-Specific Inline Documentation

- **Rust**: `///` doc comments on ALL public items, SAFETY comments for unsafe
- **Python**: Public API only, Google style format
- **TypeScript**: JSDoc with @param/@returns/@example on ALL exports
- **Java**: Javadoc on ALL public classes/methods
- **Go**: Package doc.go files, inline comments following Go conventions
- **Ruby**: YARD documentation with @param/@return tags
- **C#**: XML doc comments with `<summary>`, `<param>`, `<returns>`

## Anti-Patterns

- Missing language parity in examples
- Proactive README/documentation creation without request
- AI signatures in documentation
- Outdated examples that don't match current API
