______________________________________________________________________

## priority: critical

# Python Bindings Patterns

**Role**: Python bindings for Rust core. Work on PyO3 bridge and Python wrapper packages.

**Scope**: PyO3 FFI, Python-idiomatic API, Python-specific extensions, postprocessors.

**Commands**: maturin develop, pytest, ruff format/check.

**Critical**: Core logic lives in Rust. Only Python code for bindings, Python-specific extensions, or API wrappers. If core logic needed, coordinate with Rust team.

**Principles**: Function-based tests only, 95% coverage, builtin imports at top, no docstrings in private/test files.

## Python Modern & Performance Standards

**Python 3.10+ - Functional-first - msgspec - Fully async - Strongest typing**

- Target Python 3.10+; match/case, union types (X | Y), structural pattern matching
- msgspec ONLY (NEVER pydantic); msgspec.Struct with slots=True, kw_only=True, frozen=True
- Full type hints: ParamSpec for decorators, TypeVar/Generic[T], Protocol for structural typing
- Enable mypy --strict --warn-unreachable --disallow-any-expr; never use Any
- Functional patterns: pure functions, composition, map/filter/reduce, immutability
- Walrus operator := in comprehensions; match/case for conditionals
- contextlib.suppress for intentional exception suppression
- O(1) optimization: dict/set lookups over if/elif chains
- Fully async: anyio.Path (not pathlib), httpx AsyncClient, asyncpg, asyncio.gather
- Function-based tests ONLY (\*\_test.py); pytest fixtures, 95% coverage, real PostgreSQL
- Never: class tests, pydantic, sync I/O in async, Any type, Optional[T] (use T | None)

## PyO3 Performance Patterns

Use `pyo3_async_runtimes` for async Python callbacks (~28x faster than spawn_blocking for fast ops).

Pattern: Check `__await__` attribute, use `pyo3_async_runtimes::tokio::into_future()` for async, fallback to spawn_blocking for sync. Release GIL before awaiting. Use Python::attach() not with_gil().

spawn_blocking for long ops (OCR), block_in_place for quick ops (PostProcessor/Validator). **CRITICAL: spawn_blocking on PostProcessor/Validator causes GIL deadlocks.**
