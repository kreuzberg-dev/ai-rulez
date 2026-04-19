______________________________________________________________________

## priority: medium

- Taskfile.yaml is the primary interface for all development tasks
- task setup / task build / task test / task lint / task format / task cov:all / task bench
- Lock files always committed: uv.lock, pnpm-lock.yaml, go.sum, Cargo.lock, composer.lock
- BUILD_PROFILE supports dev/release/ci variants
- Never use manual commands instead of task, never hardcode paths, never skip lock file commits
