______________________________________________________________________

## priority: high

# Fixture Schema Design

## JSON Fixture Schema

Every fixture file contains an array of test case objects:

```json
{
  "id": "unique_snake_case_id",
  "category": "smoke|basic|parsing|edge-case|error-handling",
  "description": "Human-readable description of what this tests",
  "language": "python|rust|typescript|...",
  "source_code": "code or input to test with",
  "assertions": {
    "tree_not_null": true,
    "root_child_count_min": 1,
    "root_contains_node_type": "function_definition",
    "expect_error": false,
    "has_error_nodes": false
  },
  "skip": {
    "requires_language": "optional_language_name",
    "platform": "optional_platform"
  },
  "tags": ["smoke", "regression"]
}
```

## Schema Rules

- `id`: Unique across ALL fixture files. snake_case. Used as test function name.
- `category`: Groups related tests. Maps to test file organization in generated output.
- `assertions`: Key-value pairs defining expected outcomes. Generators translate these to language-native assertions.
- `skip`: Conditions under which the test should be skipped. Generators emit conditional skip annotations.
- `tags`: Optional labels for filtering during test runs.

## Assertion Types

- Equality: `"field_equals": {"field": "value"}`
- Contains: `"root_contains_node_type": "type_name"`
- Boolean: `"tree_not_null": true`, `"expect_error": false`
- Count: `"root_child_count_min": N`
- Type check: `"language_available": true`

## Fixture File Naming

- One category per file: `smoke.json`, `basic_parsing.json`, `error_handling.json`.
- Or organized in subdirectories: `fixtures/smoke/`, `fixtures/edge-cases/`.
