---
priority: high
---

- Rust CLI under tools/e2e-generator/ reads JSON fixtures → generates runnable test suites per language
- Generator implements Generator trait with generate() and name() per target language
- Output: e2e/{language}/ directories, each a self-contained test project
- Fixtures: JSON under tools/e2e-generator/fixtures/ by category (smoke, basic, parsing, edge-case, error-handling)
- Each fixture has unique snake_case id, loaded recursively, sorted by (category, id)
- Run task generate:e2e to regenerate, task test:e2e to verify
- Never hand-edit generated files — modify fixtures or generator source instead
