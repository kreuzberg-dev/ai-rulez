______________________________________________________________________

## priority: medium

# Monitoring & Observability Standards

## Structured Logging

Use `tracing` crate (Rust) or `structlog` (Python). Key=value pairs, not f-strings. JSON output for aggregation.

- `#[instrument]` macro on functions for automatic span creation
- `EnvFilter` for runtime log level control
- Span fields for context: `user_id`, `request_id`, etc.

## Log Levels

| Level | Use For |
|-------|---------|
| ERROR | Unrecoverable failures (DB down, invalid request) |
| WARN | Degraded state, deprecated usage |
| INFO | State changes, milestones (startup, migration complete) |
| DEBUG | Execution flow, variable values |
| TRACE | Detailed loops/assignments (off in production) |

## Metrics (Prometheus)

- **Counter**: Monotonic (requests, errors)
- **Gauge**: Current value (connections, queue size)
- **Histogram**: Distributions (latency, payload size)

Avoid high-cardinality labels (no user_id in label values).

## Health Checks

Expose `/health` endpoint returning JSON status with component checks. Wire to Kubernetes liveness/readiness probes.

## Anti-Patterns

- f-string logging instead of structured key=value
- No span context on request handling
- Logging sensitive data (passwords, tokens, PII)
- Unbounded label cardinality in metrics
- No health check endpoints
