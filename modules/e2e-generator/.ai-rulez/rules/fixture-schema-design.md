---
priority: high
---

- Fixture fields: id (unique snake_case), category, description, language, source_code, assertions, skip, tags
- Assertion types: equality (field_equals), contains (root_contains_node_type), boolean (tree_not_null), count (root_child_count_min)
- Skip conditions: requires_language, platform — generators emit language-native skip annotations
- IDs unique across ALL fixture files, used as test function names
- One category per file (smoke.json) or organized in subdirectories (fixtures/smoke/)
