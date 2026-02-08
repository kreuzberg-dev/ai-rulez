______________________________________________________________________

## priority: medium

# Containerization & Docker Standards

## Multi-Stage Builds

- **Builder stage**: Full Rust toolchain + deps, compile with `--release --target x86_64-unknown-linux-musl`
- **Runtime stage**: Minimal base (Alpine 5MB, Distroless 20MB, or Scratch 0B for static binaries)
- Layer caching: copy `Cargo.toml`/`Cargo.lock` first, then source

## Security

- Run as non-root user (`adduser`)
- Scan with Trivy/Grype in CI, fail on HIGH/CRITICAL
- No secrets in image (use env vars or secret mounts)
- Read-only filesystem when possible

## Best Practices

- `tini`/`dumb-init` as PID 1 for signal handling
- `HEALTHCHECK` for orchestrator detection
- Immutable tags (use commit SHA, never reuse tags)
- BuildKit mount caches for cargo registry: `--mount=type=cache,target=/usr/local/cargo/registry`
- GHA cache: `cache-from: type=gha`, `cache-to: type=gha,mode=max`

## Image Tagging

Semantic version + latest + branch + commit SHA. Example: `myapp:1.2.3`, `myapp:latest`, `myapp:abc1234f`

## Anti-Patterns

- Running as root
- Large base images (ubuntu instead of alpine/distroless)
- Single-stage builds
- Not using BuildKit
- No HEALTHCHECK
- Hardcoding secrets
