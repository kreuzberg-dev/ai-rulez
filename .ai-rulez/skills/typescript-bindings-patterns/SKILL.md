______________________________________________________________________

## priority: critical

# TypeScript Bindings Patterns

**Role**: TypeScript bindings for Rust core. Work on NAPI-RS bridge and TypeScript SDK packages.

**Scope**: NAPI-RS FFI, TypeScript-idiomatic API, type definitions, JSDoc for all exports with @param/@returns/@example.

**Commands**: pnpm install/build/test/lint.

**Critical**: Core logic lives in Rust. TypeScript only for bindings/wrappers. If core logic needed, coordinate with Rust team.

## TypeScript Strictest Standards

**TypeScript 5.x - Strictest typing - No any/object - Generics required - Tests next to source**

- Enable ALL strict flags: strict, noUncheckedIndexedAccess, exactOptionalPropertyTypes
- Ban any and object types; use unknown with guards, Record\<string, unknown>
- Generics with constraints: <T extends BaseType>, satisfies operator, const assertions
- Tests: .spec.ts next to source files (NOT __tests__/); vitest, 80%+ coverage
- Functional: pure functions over classes, map/filter/reduce, immutability, readonly
- Nullish coalescing ??, optional chaining ?., type predicates (x is Type)
- Import type for types, organize by feature, path aliases (@/lib/\*)
- Biome for linting/formatting, pnpm >=10.17, pnpm-lock.yaml committed
- React: function components, custom hooks (use\*), proper prop typing
- Never: any/object types, __test__ dirs, non-null assertions !, || for defaults
