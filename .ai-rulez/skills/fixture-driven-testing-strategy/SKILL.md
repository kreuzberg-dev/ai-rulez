---
priority: high
---

# Fixture-Driven Testing Strategy

## Shared Test Fixtures Across Language Bindings

- **Single source of truth**: Fixtures defined once in Rust, exposed to all language bindings
- **API parity validation**: Identical behavior across languages guaranteed by fixtures
- **Fixture generation**: Rust generates JSON/YAML fixtures from canonical implementation
- **Language-specific consumption**: Each binding consumes same fixtures via fixture format (JSON, YAML, MessagePack)

Example directory structure:

```
project/
├── crates/
│   ├── core/
│   │   ├── src/
│   │   ├── tests/
│   │   └── fixtures/
│   │       ├── basic.json
│   │       ├── edge_cases.json
│   │       └── generator.rs  # Generates fixtures from canonical impl
│   └── ffi/
│       └── src/lib.rs
├── bindings/
│   ├── python/
│   │   ├── tests/
│   │   │   ├── conftest.py  # Load fixtures
│   │   │   └── test_*.py
│   │   └── fixtures -> ../../crates/core/fixtures  # Symlink
│   ├── node/
│   │   ├── __tests__/
│   │   └── fixtures -> ../../crates/core/fixtures
│   ├── ruby/
│   │   ├── spec/
│   │   └── fixtures -> ../../crates/core/fixtures
│   ├── java/
│   │   ├── src/test/
│   │   └── resources/fixtures -> ../../crates/core/fixtures
│   └── go/
│       ├── *_test.go
│       └── testdata/fixtures -> ../../crates/core/fixtures
```

## Fixture Formats

### JSON Format (Primary)

```json
{
  "version": 1,
  "generated_at": "2025-01-15T10:30:00Z",
  "test_cases": [
    {
      "name": "basic_conversion",
      "input": {
        "html": "<h1>Hello</h1><p>World</p>"
      },
      "expected_output": {
        "markdown": "# Hello\n\nWorld"
      },
      "description": "Simple HTML to Markdown conversion"
    },
    {
      "name": "nested_lists",
      "input": {
        "html": "<ul><li>Item 1<ul><li>Nested</li></ul></li></ul>"
      },
      "expected_output": {
        "markdown": "- Item 1\n  - Nested"
      }
    },
    {
      "name": "table_conversion",
      "input": {
        "html": "<table><tr><th>Header</th></tr><tr><td>Data</td></tr></table>"
      },
      "expected_output": {
        "markdown": "| Header |\n|--------|\n| Data   |"
      }
    },
    {
      "name": "edge_case_empty_input",
      "input": {
        "html": ""
      },
      "expected_output": {
        "markdown": ""
      }
    },
    {
      "name": "error_case_invalid_html",
      "input": {
        "html": "<div>Unclosed tag"
      },
      "expected_error": "malformed_html"
    }
  ]
}
```

### YAML Format (Human-Readable)

```yaml
version: 1
generated_at: 2025-01-15T10:30:00Z

test_cases:
  - name: basic_conversion
    input:
      html: |
        <h1>Hello</h1>
        <p>World</p>
    expected_output:
      markdown: |
        # Hello

        World

  - name: nested_lists
    input:
      html: |
        <ul>
          <li>Item 1
            <ul>
              <li>Nested</li>
            </ul>
          </li>
        </ul>
    expected_output:
      markdown: |
        - Item 1
          - Nested

  - name: special_chars
    input:
      html: "<p>Test & verify \"quotes\"</p>"
    expected_output:
      markdown: "Test & verify \"quotes\""

  - name: unicode_handling
    input:
      html: "<p>Hello 世界 🌍</p>"
    expected_output:
      markdown: "Hello 世界 🌍"
```

## Fixture Generation from Rust

Generate fixtures programmatically from Rust canonical implementation:

```rust
// crates/core/src/lib.rs - Canonical implementation
pub fn html_to_markdown(html: &str) -> Result<String> {
    // Core conversion logic
    // ...
}

#[cfg(test)]
mod fixture_generation {
    use super::*;
    use serde_json::json;
    use std::fs;

    #[test]
    #[ignore]  // Run manually with: cargo test -- --ignored --nocapture
    fn generate_fixtures() {
        let test_cases = vec![
            ("basic_conversion", "<h1>Hello</h1><p>World</p>"),
            ("nested_lists", "<ul><li>Item 1<ul><li>Nested</li></ul></li></ul>"),
            ("table_conversion", "<table><tr><th>Header</th></tr><tr><td>Data</td></tr></table>"),
            ("empty_input", ""),
        ];

        let mut fixtures = json!({
            "version": 1,
            "generated_at": chrono::Utc::now().to_rfc3339(),
            "test_cases": []
        });

        let cases = fixtures["test_cases"].as_array_mut().unwrap();

        for (name, html) in test_cases {
            match html_to_markdown(html) {
                Ok(markdown) => {
                    cases.push(json!({
                        "name": name,
                        "input": { "html": html },
                        "expected_output": { "markdown": markdown }
                    }));
                }
                Err(e) => {
                    cases.push(json!({
                        "name": name,
                        "input": { "html": html },
                        "expected_error": e.to_string()
                    }));
                }
            }
        }

        let output_path = "crates/core/fixtures/canonical.json";
        fs::write(
            output_path,
            serde_json::to_string_pretty(&fixtures).unwrap()
        ).unwrap();

        println!("Generated fixtures to {}", output_path);
    }
}
```

Usage:
```bash
cargo test --package core -- --ignored fixture_generation --nocapture
```

## Python Fixture Consumption

```python
import json
import pytest
from pathlib import Path

@pytest.fixture(scope="session")
def fixtures():
    """Load shared test fixtures from JSON"""
    fixture_path = Path(__file__).parent.parent.parent / "crates" / "core" / "fixtures" / "canonical.json"
    with open(fixture_path) as f:
        return json.load(f)

@pytest.mark.parametrize("test_case", fixtures()["test_cases"], ids=lambda tc: tc["name"])
def test_html_to_markdown(test_case):
    """Test against shared fixtures"""
    from html_to_markdown import convert

    input_html = test_case["input"]["html"]

    if "expected_error" in test_case:
        with pytest.raises(Exception) as exc_info:
            convert(input_html)
        assert test_case["expected_error"] in str(exc_info.value)
    else:
        expected_markdown = test_case["expected_output"]["markdown"]
        actual_markdown = convert(input_html)
        assert actual_markdown == expected_markdown

    # Display test metadata
    if "description" in test_case:
        print(f"\n{test_case['description']}")
```

## TypeScript/Node.js Fixture Consumption

```typescript
import * as fs from "fs";
import { htmlToMarkdown } from "../src";

interface TestCase {
  name: string;
  input: { html: string };
  expected_output?: { markdown: string };
  expected_error?: string;
  description?: string;
}

const fixtures: TestCase[] = JSON.parse(
  fs.readFileSync(
    "../../crates/core/fixtures/canonical.json",
    "utf-8"
  )
).test_cases;

describe("Fixture-driven tests", () => {
  fixtures.forEach((testCase) => {
    it(`${testCase.name}: ${testCase.description || ""}`, async () => {
      if (testCase.expected_error) {
        expect(() => htmlToMarkdown(testCase.input.html)).toThrow(
          testCase.expected_error
        );
      } else {
        const result = htmlToMarkdown(testCase.input.html);
        expect(result).toBe(testCase.expected_output!.markdown);
      }
    });
  });
});
```

## Ruby Fixture Consumption

```ruby
require 'json'
require 'rspec'
require 'html_to_markdown'

fixtures = JSON.parse(
  File.read(File.expand_path('../../../../crates/core/fixtures/canonical.json', __FILE__))
)

describe HtmlToMarkdown do
  fixtures['test_cases'].each do |test_case|
    context test_case['name'] do
      it test_case['description'] || 'converts correctly' do
        input_html = test_case['input']['html']

        if test_case['expected_error']
          expect { HtmlToMarkdown.convert(input_html) }.to raise_error
        else
          expected_markdown = test_case['expected_output']['markdown']
          actual_markdown = HtmlToMarkdown.convert(input_html)
          expect(actual_markdown).to eq(expected_markdown)
        end
      end
    end
  end
end
```

## Java Fixture Consumption

```java
import com.google.common.io.Resources;
import com.google.gson.Gson;
import com.google.gson.JsonObject;
import com.google.gson.JsonArray;
import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;

public class HtmlToMarkdownFixtureTest {

    static class TestCase {
        String name;
        Input input;
        Output expected_output;
        String expected_error;
    }

    static class Input {
        String html;
    }

    static class Output {
        String markdown;
    }

    static Stream<TestCase> loadFixtures() throws Exception {
        String fixturesJson = Resources.toString(
            Resources.getResource("fixtures/canonical.json"),
            StandardCharsets.UTF_8
        );

        Gson gson = new Gson();
        JsonObject root = gson.fromJson(fixturesJson, JsonObject.class);
        JsonArray testCases = root.getAsJsonArray("test_cases");

        return StreamSupport.stream(testCases.spliterator(), false)
            .map(elem -> gson.fromJson(elem, TestCase.class));
    }

    @ParameterizedTest(name = "{0}")
    @MethodSource("loadFixtures")
    void testHtmlToMarkdown(TestCase testCase) {
        if (testCase.expected_error != null) {
            assertThrows(Exception.class, () ->
                HtmlToMarkdown.convert(testCase.input.html)
            );
        } else {
            String result = HtmlToMarkdown.convert(testCase.input.html);
            assertEquals(testCase.expected_output.markdown, result);
        }
    }
}
```

## Snapshot Testing

Use snapshot testing for complex outputs (diffs shown in PRs):

```rust
// Rust with insta crate
use insta::assert_snapshot;

#[test]
fn test_complex_html_snapshot() {
    let html = r#"
        <article>
            <h1>Title</h1>
            <section>
                <h2>Subtitle</h2>
                <p>Content</p>
            </section>
        </article>
    "#;

    let result = html_to_markdown(html).unwrap();
    assert_snapshot!(result);
}
```

```python
# Python with pytest-snapshot or syrupy
def test_complex_html_snapshot(snapshot):
    html = """
        <article>
            <h1>Title</h1>
            <section>
                <h2>Subtitle</h2>
                <p>Content</p>
            </section>
        </article>
    """
    result = convert(html)
    assert result == snapshot
```

```typescript
// TypeScript with jest snapshots
test("complex HTML snapshot", () => {
  const html = `
    <article>
        <h1>Title</h1>
        <section>
            <h2>Subtitle</h2>
            <p>Content</p>
        </section>
    </article>
  `;

  expect(htmlToMarkdown(html)).toMatchSnapshot();
});
```

## Cross-Language Test Consistency

Ensure identical behavior across bindings:

```bash
#!/bin/bash
# test_parity.sh - Run fixture tests across all bindings and compare

set -e

FIXTURE_FILE="crates/core/fixtures/canonical.json"

echo "Testing Rust (canonical)..."
cargo test --package core --test fixture_driven

echo "Testing Python..."
cd bindings/python && python -m pytest tests/test_fixtures.py -v && cd ../..

echo "Testing Node.js..."
cd bindings/node && npm test -- test/fixtures.test.ts && cd ../..

echo "Testing Ruby..."
cd bindings/ruby && bundle exec rspec spec/fixtures_spec.rb && cd ../..

echo "Testing Java..."
cd bindings/java && ./gradlew test --tests "*FixtureTest" && cd ../..

echo "All fixture tests passed!"
```

Run in CI:
```yaml
- name: Fixture parity test
  run: bash scripts/test_parity.sh
```

## Anti-Patterns

- **Language-specific fixtures**: Define once in Rust, reuse everywhere
- **Hardcoded test data**: Use fixture files for maintainability
- **No error case testing**: Include `expected_error` cases in fixtures
- **Incomplete fixtures**: Cover edge cases, unicode, special characters
- **Manual fixture updates**: Generate from canonical implementation automatically
- **No fixture versioning**: Include version field to track breaking changes
- **Test duplication**: Write fixture consumer once per language, parameterize tests
- **Snapshot conflicts in PR**: Use deterministic snapshot naming
- **No fixture documentation**: Include `description` field explaining intent
