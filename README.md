# AI-Rulez

Shared AI governance modules for kreuzberg.dev polyglot projects. Each module is independently includable via the `path` field on includes.

## Available Modules

### `modules/core` (~17K)

Polyglot Rust + multi-language binding conventions. **All repos need this.**

- Rules: binding crate architecture, FFI interop, polyglot error handling, Rust conventions
- Agents: code-reviewer, ffi-maintenance-engineer, polyglot-architect
- Skills: common-task-commands, quick-start, universal-anti-patterns, polyglot-bindings, polyglot-api-documentation, ci-cd-multi-platform-matrix
- Context: polyglot architecture overview

### `modules/cicd` (~8K)

CI/CD pipelines, task automation, and release processes. **Most repos need this.**

- Rules: CI/CD pipeline standards, GitHub workflows, task automation
- Agents: release-versioning-coordinator
- Skills: modular-taskfile-structure, release-and-deployment-processes
- Context: RTK overview, tooling overview

### `modules/e2e-generator` (~12K)

Fixture-driven cross-language test generation. **Repos with `tools/e2e-generator/`.**

- Rules: e2e generator conventions, fixture schema design, generated code policy
- Agents: e2e-generator-engineer
- Skills: create-e2e-fixture, add-language-generator
- Context: e2e generator architecture

### `modules/infrastructure` (~7K)

Container, cloud, and monitoring standards. **Only repos with deployed services.**

- Rules: containerization/Docker, GCloud conventions, monitoring/observability
- Agents: devops-infrastructure-engineer
- Skills: containerization-docker-standards, monitoring-observability-standards

### `modules/tooling` (~4K)

Optional tooling-specific rules (GWS, Playwright MCP, RTK usage).

### `modules/architecture` (~88K)

Architecture reference docs, design documents, and ADRs. **Large — include only when needed.**

## Usage

Include specific modules in your project's `config.yaml`:

```yaml
includes:
  - name: kreuzberg-core
    source: https://github.com/kreuzberg-dev/ai-rulez.git
    path: modules/core
    merge_strategy: local-override
  - name: kreuzberg-cicd
    source: https://github.com/kreuzberg-dev/ai-rulez.git
    path: modules/cicd
    merge_strategy: local-override
```

Each module is a self-contained ai-rulez structure with `rules/`, `context/`, `skills/`, and `agents/` subdirectories.

## Consumer Configurations

| Repo | Modules |
|------|---------|
| kreuzberg | core, cicd, infrastructure, e2e-generator |
| spikard | core, cicd, e2e-generator |
| tree-sitter-language-pack | core, cicd, e2e-generator |
| html-to-markdown | core, cicd |

## License

See the LICENSE file for licensing information.
