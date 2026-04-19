______________________________________________________________________

## priority: medium

- Logging: tracing crate (Rust) / structlog (Python), JSON output, key=value — never f-strings
- Spans: #[instrument] macro, context fields (user_id, request_id)
- Levels: ERROR (unrecoverable), WARN (degraded), INFO (state changes), DEBUG (flow), TRACE (off in prod)
- Metrics: Prometheus — counter (requests), gauge (connections), histogram (latency), no high-cardinality labels
- Health: /health endpoint with component status, wired to K8s liveness/readiness probes
