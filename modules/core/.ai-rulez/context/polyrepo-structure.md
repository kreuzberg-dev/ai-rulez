---
priority: high
---

# Polyrepo Structure

This repo is part of the `kreuzberg-dev` polyrepo. From any subrepo, `../` is the polyrepo root.

- Shared AI governance config: `../ai-rulez/` (git submodule with shared modules)
- Polyrepo-level orchestration: `../Taskfile.yml`, `../.pre-commit-config.yaml`
- Each subrepo is an independent git repo with its own `.ai-rulez/config.yaml`
- Navigate between repos via `../` relative paths
