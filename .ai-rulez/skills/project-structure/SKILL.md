______________________________________________________________________

## priority: medium

# Project Structure & Conventions

```
html-to-markdown/
├── crates/
│   ├── html-to-markdown/           # Core Rust library (main logic)
│   ├── html-to-markdown-py/        # PyO3 bindings for Python
│   ├── html-to-markdown-node/      # NAPI-RS bindings for Node.js
│   ├── html-to-markdown-rb/        # Magnus bindings for Ruby
│   ├── html-to-markdown-php/       # ext-php-rs extension for PHP
│   ├── html-to-markdown-wasm/      # wasm-bindgen for WebAssembly
│   ├── html-to-markdown-ffi/       # C FFI library (Go, Java, C# wrapper)
│   ├── html-to-markdown-cli/       # Standalone CLI binary
│   └── Cargo.toml                  # Workspace manifest
│
├── packages/
│   ├── python/
│   │   ├── src/                    # Python package source
│   │   ├── tests/                  # pytest test suite
│   │   └── pyproject.toml
│   ├── typescript/
│   │   ├── src/                    # TypeScript source
│   │   ├── tests/                  # vitest test suite
│   │   └── package.json
│   ├── ruby/
│   │   ├── lib/                    # Ruby source
│   │   ├── sig/                    # RBS type definitions
│   │   ├── spec/                   # RSpec test suite
│   │   └── Gemfile
│   ├── php/
│   │   ├── src/                    # PHP source
│   │   ├── tests/                  # PHPUnit test suite
│   │   └── composer.json
│   ├── php-ext/                    # PIE packaging metadata
│   ├── go/                         # Go module wrapper
│   ├── java/                       # Maven Java project
│   └── csharp/                     # .NET C# project
│
├── examples/
│   ├── fixtures/                   # HTML test fixtures (JSON/YAML)
│   ├── visitor-pattern/            # Visitor pattern guide + examples
│   ├── metadata-extraction/        # Metadata extraction guide + examples
│   └── performance/                # Performance guide + benchmarks
│
├── tools/
│   ├── benchmark-harness/          # Rust + binding benchmark harness
│   └── ...
│
├── scripts/
│   ├── sync_versions.py            # Sync version across all manifests
│   ├── prepare_wheel.py            # Python wheel prep
│   ├── package_php_pie_source.sh   # PHP PIE packaging
│   └── preferred-rustc.sh          # WASM rustc selection
│
├── .github/workflows/
│   ├── ci-rust.yaml                # Rust unit + coverage
│   ├── ci-python.yaml              # Python binding + CLI build/tests
│   ├── ci-node.yaml                # Node/TypeScript bindings + tests
│   ├── ci-wasm.yaml                # WASM builds and Wasmtime tests
│   ├── ci-ruby.yaml                # Ruby bindings
│   ├── ci-php.yaml                 # PHP bindings
│   ├── ci-go.yaml                  # Go linting (golangci-lint)
│   ├── ci-java.yaml                # Java Panama bindings
│   ├── ci-elixir.yaml              # Elixir bindings
│   └── ci-validate.yaml            # Lint/format/prek validation
│   └── test-wheels.yaml            # Python wheel testing
│
├── Taskfile.yaml                   # Task automation (PRIMARY DEV INTERFACE)
├── Cargo.toml                      # Cargo workspace
├── Cargo.lock                      # Lock file (committed)
├── pnpm-lock.yaml                  # pnpm lock file (committed)
├── .prek-config.yaml               # Prek pre-commit hooks
└── ai-rulez.yaml                   # This file (AI guidelines)
```

## Workspace Structure & Project Organization

**Rust workspace** (Cargo.toml): crates/{kreuzberg,kreuzberg-py,kreuzberg-node,kreuzberg-ffi,kreuzberg-cli}, packages/ruby/ext/kreuzberg_rb/native, tools/{benchmark-harness,e2e-generator}, e2e/{rust,go}.

**Language packages**: packages/{python,typescript,ruby,java,go} - thin wrappers around Rust core.

**E2E tests**: Auto-generated from fixtures/ via tools/e2e-generator. Located in e2e/{rust,python,typescript,ruby,java,go}.

**Benchmarking**: Rust harness in tools/benchmark-harness.

**Install**: `uv sync --all-extras --all-packages --all-groups`.

**Version sync**: `task sync-versions` (syncs from Cargo.toml to all manifests).
