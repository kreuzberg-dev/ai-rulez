---
description: "Add a new language generator to the e2e test generation system"
priority: medium
---

# Add Language Generator

## Steps

1. **Create generator module**: `tools/e2e-generator/src/generators/{language}.rs`
1. **Implement Generator trait**:
   ```rust
   pub struct {Language}Generator;

   impl Generator for {Language}Generator {
       fn name(&self) -> &'static str { "{language}" }
       fn generate(&self, fixtures: &[Fixture], output_dir: &Path) -> Result<(), String> {
           // Generate test files
       }
   }
   ```
1. **Add string escaping**: Implement `escape_{language}(s: &str) -> String` for the target language's string literal rules.
1. **Generate project scaffold**: Dependency file (Cargo.toml, pyproject.toml, etc.), test runner config, helper utilities.
1. **Register in dispatcher**: Add to `src/generators/mod.rs` generator list.
1. **Test output**: Run generator, verify tests compile and pass.
1. **Update CI matrix**: Add the new language to the e2e test CI workflow.

## Checklist

- [ ] Generator produces deterministic output
- [ ] String escaping handles: newlines, quotes, backslashes, unicode
- [ ] Project scaffold includes all dependencies
- [ ] Generated tests use language-idiomatic assertion patterns
- [ ] Skip conditions translate to language-native skip annotations
- [ ] CI runs generated tests on all supported platforms
