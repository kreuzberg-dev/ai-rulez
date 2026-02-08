______________________________________________________________________

## priority: high

# Elixir Bindings Patterns (Rustler NIF)

**Role**: Elixir bindings for Rust core using Rustler NIF bridge pattern.

**Scope**:

- Rust NIF bridge: packages/elixir/native/<project>\_rustler/ (Rust crate with cdylib output)
- Elixir wrapper: packages/elixir/lib/<project>/ (OTP application with public API)
- ExUnit tests: packages/elixir/test/

**Architecture**:
Elixir OTP application → Rustler NIF (.so/.dylib) → Rust core

Data flow: Elixir terms → term_to_json → serde_json::Value → Core API → serde_json::Value → json_to_term → Elixir terms

**Commands**:

- mix deps.get (fetch dependencies)
- mix compile (compile Elixir + Rustler NIF)
- mix test (run ExUnit tests)
- mix credo (lint with Credo)
- mix format (format code)
- mix docs (generate ExDoc documentation)

**Build System**:

- mix.exs: Elixir project configuration with Rustler dependency
- Native crate: packages/elixir/native/<project>\_rustler/Cargo.toml
- Compiled NIF: priv/native/<project>\_rustler.so (loaded at runtime)
- Workspace exclusion: Native crate excluded from main Cargo workspace

**Critical**:

- Core logic lives in Rust. Elixir only for bindings/wrappers and OTP integration.
- If core logic changes needed, coordinate with Rust team.
- Rustler handles serialization between Erlang terms and Rust types (NifMap, Binary, ResourceArc).
- Use dirty schedulers for CPU-intensive work to avoid blocking BEAM schedulers.
- Resource cleanup: Use ResourceArc for Rust objects that need garbage collection.

**NIF Patterns**:

- rustler::init!() macro registers NIFs with BEAM VM
- #[rustler::nif] attribute marks functions as NIFs
- #[rustler::nif(schedule = "DirtyCpu")] for CPU-intensive work
- Field-by-field map construction: rustler::types::map::map_new() + incremental map_put() (NO NifMap derive)
- term_to_json helper: Elixir term → serde_json::Value (handles atoms, booleans, numbers, strings, lists, maps)
- json_to_term helper: serde_json::Value → Elixir term (recursive conversion for nested structures)
- OwnedBinary + Binary for efficient binary data: OwnedBinary::new() + Binary::from_owned()
- ResourceArc<T> for Rust objects with GC integration
- Dual-path config parsing: serde_json deserialization + explicit field handling for boolean fields

**Config Parsing Approach**:

1. Accept Elixir map with atom/string keys via Term parameter
1. Convert term → serde_json::Value using term_to_json helper
1. Deserialize using serde_json::from_value() for nested structures
1. Explicitly handle top-level booleans for compatibility
1. Return default config if parsing fails at any step

**Documentation**:

- All public modules and functions documented with ExDoc (@moduledoc, @doc)
- Include @spec annotations for all exported functions
- Examples in module documentation for common use cases
- README.md with installation, usage, and API overview

## Elixir 1.14+ Standards (ExDoc & Credo)

- Target Elixir 1.14+ with .tool-versions file; use ASDF for version management
- Module structure: CamelCase for modules, snake_case for functions/variables
- ExDoc for documentation: @moduledoc and @doc tags on all public APIs with examples
- ExUnit for testing: describe/test blocks, 80%+ test coverage, property-based testing with StreamData
- Credo for linting: strict mode enabled, no ignored warnings
- Type specifications: @spec, @type, @callback annotations on all public functions
- Guard clauses and pattern matching for control flow (avoid if/else chains)
- OTP principles: Supervision trees, GenServer for state, Agent for simple storage
- Erlang/OTP interoperability: understand BEAM VM concepts (processes, mailboxes, schedulers)
- Result tuples: {:ok, value} and {:error, reason} patterns (NEVER exceptions for control flow)
- Immutable data structures: pipelines with |> operator for transformations
- Pure functions: side effects isolated to GenServer/Agent/NIFs
- Code quality: Functions concise (\<20 lines), guard clauses, modules for logical organization
- Binary handling: Use Erlang binaries efficiently, avoid unnecessary copying
- CI: mix credo --strict, mix test with coverage, mix format --check-formatted, mix docs
