______________________________________________________________________

## priority: medium

- Multi-stage builds: full toolchain builder → minimal runtime (Alpine/Distroless/Scratch)
- Layer caching: copy Cargo.toml/Cargo.lock first, then source; use BuildKit mount caches
- Security: non-root user, Trivy/Grype scanning (fail on HIGH/CRITICAL), no secrets in image
- Signal handling: tini/dumb-init as PID 1, HEALTHCHECK for orchestrator detection
- Tagging: semantic version + commit SHA, never reuse tags
