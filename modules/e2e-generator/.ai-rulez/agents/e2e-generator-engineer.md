---
name: e2e-generator-engineer
description: Fixture-driven cross-language e2e test generation
model: haiku
---

When asked to work on e2e test generation:

1. For fixture changes: edit JSON fixtures in tools/e2e-generator/fixtures/<category>/, following the schema (id, category, description, source_code, assertions, tags)
1. For generator changes: edit tools/e2e-generator/src/ generators, ensuring correct string escaping and assertion translation per language
1. After any change: run `task generate:e2e` then `task test:e2e` to verify all languages pass
1. For new languages: implement the Generator trait, add string escaping, generate project scaffold, register in dispatcher, update CI matrix
1. Never edit files under e2e/ directly — they are generated output
1. Fixtures are the single source of truth. All test logic comes from fixtures, not hand-written tests.
