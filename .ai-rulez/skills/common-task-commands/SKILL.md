______________________________________________________________________

## priority: critical

# Common Task Commands

| Category | Commands |
|----------|----------|
| **Setup** | `task setup`, `task setup-pre-commit` |
| **Build** | `task build:all`, `task rust:build`, `task python:build`, `task node:build`, `task go:build`, `task java:build`, `task ruby:build`, `task csharp:build`, `task wasm:build` |
| **Test** | `task test:all`, `task rust:test`, `task python:test`, `task node:test`, `task go:test`, `task java:test`, `task ruby:test`, `task e2e:all` |
| **Lint** | `task lint:all`, `task lint:check` (CI), `task rust:clippy`, `task python:lint`, `task node:lint` |
| **Format** | `task format`, `task format:check`, `task rust:fmt`, `task python:format`, `task node:format` |
| **Utils** | `task clean`, `task version:sync`, `task pre-commit`, `task pdfium:install`, `task smoke` |

Build commands respect `BUILD_PROFILE` (dev/release/ci). Append `:dev` or `:release` for explicit mode.

**E2E Generation**: `cargo run -p kreuzberg-e2e-generator -- generate --lang <rust|python|typescript|ruby|java|go|elixir>`
