______________________________________________________________________

## priority: high

# Go Bindings Patterns

**Role**: Go bindings for Rust core. Work on CGO bridge and Go SDK/E2E suite.

**Scope**: Go 1.25 module, CGO wrappers around C FFI, Go-idiomatic config/result structs, linting setup, benchmark harness scripts.

**Commands**: cd packages/go/v4 && go test ./..., golangci-lint run ./..., ensure `LD_LIBRARY_PATH`/`DYLD_LIBRARY_PATH` includes target/release when running tests.

**FFI**: Binding header (packages/go/v4/binding.go) must stay in sync with C FFI header. Add new APIs to Rust first, then expose through CGO, update types.go, regenerate Go E2E tests when fixtures change.

**Critical**: Core logic lives in Rust. Go code should remain thin wrappers/helper utilities over C API. Coordinate with Rust team for shared logic.

## Go 1.25+ Standards

- Go 1.25+; error wrapping with fmt.Errorf("%w", err), errors.Is/As for checking
- Testing: \*\_test.go with \_test package suffix (black-box), table-driven with t.Run()
- golangci-lint: errcheck, govet, staticcheck, gosec, gocyclo (complexity \<=25)
- Coverage 80%+ on business logic; go test -race for concurrency bugs
- Package structure: cmd/, internal/ (no cross-service imports), pkg/ for libraries
- Naming: PascalCase (types), camelCase (vars), SCREAMING_SNAKE_CASE (consts)
- Context.Context first param in I/O funcs; respect cancellation with select \<-ctx.Done()
- Structured logging: zerolog with Str/Int/Err chaining
- Never: bare error returns, cross-service internal imports, panic in libraries
