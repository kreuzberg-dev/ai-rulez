---
priority: critical
---

# Cross-Ecosystem Version Synchronization

**Single source of truth · Automated sync · Monorepo consistency · CI verification**

## Core Principle

Cargo.toml `workspace.package.version` is the authoritative version source for all packages across all language ecosystems. All other package manifests (pyproject.toml, package.json, pom.xml, go.mod, Gemfile, mix.exs, composer.json, *.csproj) must be derived from this single source.

## Single Source of Truth

- **Cargo.toml workspace.package.version**: Primary version definition
  ```toml
  [workspace.package]
  version = "0.8.2"
  ```
- All packages in the monorepo share the same version (no independent versioning)
- Version bumps occur exclusively in Cargo.toml
- Never manually edit language-specific package manifests for versions

## Version Sync Targets

### Cargo Ecosystem
- `Cargo.toml`: Each workspace member inherits via `version.workspace = true`
- All crates must reference workspace version

### Python Ecosystem
- `pyproject.toml`: `version` field under `[project]` section
- Example sync: `version = "0.8.2"`

### JavaScript/TypeScript Ecosystem
- `package.json`: `version` field in root and all workspace packages
- `package.json` root: matches workspace version
- All packages under `packages/typescript/` use same version

### Java/Maven
- `pom.xml`: `<version>` tag in `<project>` element
- Parent POM version management approach

### Go Ecosystem
- `go.mod`: Module versioning via semantic versioning tags
- Release tags: `v0.8.2` (with v prefix)

### Ruby Ecosystem
- `Gemfile`: Specify version via gemspec or direct version management
- `*.gemspec`: `spec.version = "0.8.2"`

### PHP Ecosystem
- `composer.json`: `"version"` field in JSON root
- Synchronize across all PHP packages

### .NET/C# Ecosystem
- `*.csproj`: `<Version>` property in PropertyGroup
- Example: `<Version>0.8.2</Version>`

### Elixir Ecosystem
- `mix.exs`: `@version "0.8.2"` module attribute
- All Elixir packages use same version

## Monorepo Versioning Strategy

### All Packages Same Version
- **Requirement**: Every package released in the monorepo uses identical version number
- **Rationale**: Simplifies dependency management, ensures consistency across ecosystem integrations
- **Release Cadence**: All packages bump version on same release date
- **No Independent Versioning**: Individual language packages cannot have independent version numbers

### Version Advancement Rules
1. Major (X.0.0): Breaking API changes across any ecosystem
2. Minor (0.X.0): New features added to any package
3. Patch (0.0.X): Bug fixes, security updates, or non-breaking improvements

## Automated Synchronization

### task sync-versions Command

Execute before every release:

```bash
task sync-versions
```

This task automates:

1. **Read** Cargo.toml workspace version
2. **Update** pyproject.toml `[project]` version
3. **Update** package.json root and all workspace members
4. **Update** pom.xml version across all modules
5. **Update** go.mod module declaration
6. **Update** gemspec version strings
7. **Update** composer.json version field
8. **Update** *.csproj Version properties
9. **Update** mix.exs @version attribute
10. **Verify** all files updated successfully
11. **Report** summary of changed files

### Implementation Requirements

- **scripts/sync_versions.py**: Primary synchronization script
  - Reads Cargo.toml workspace version
  - Updates all ecosystem-specific manifests
  - Validates against glob patterns for each ecosystem
  - Exits with error code on failure
  - Generates report of all changes

- **Taskfile.yaml sync-versions task**:
  ```yaml
  sync-versions:
    desc: "Synchronize version across all package ecosystems"
    cmds:
      - python scripts/sync_versions.py
  ```

- **Safety checks**:
  - Dry-run mode: `task sync-versions -- --dry-run`
  - Validation that all target files exist
  - Rollback on partial failure
  - Git diff review before commit

## CI Verification Requirements

### Pre-Merge Verification

CI must verify version consistency before merge:

```yaml
- name: Verify Version Consistency
  run: |
    CARGO_VERSION=$(grep -A1 'workspace.package' Cargo.toml | grep version | sed 's/.*version = "//' | sed 's/".*//')
    PYTHON_VERSION=$(grep '^version' pyproject.toml | head -1 | sed 's/.*= "//' | sed 's/".*//')
    NODE_VERSION=$(jq -r '.version' package.json)

    if [[ "$CARGO_VERSION" != "$PYTHON_VERSION" ]]; then
      echo "ERROR: Cargo ($CARGO_VERSION) != Python ($PYTHON_VERSION)"
      exit 1
    fi

    if [[ "$CARGO_VERSION" != "$NODE_VERSION" ]]; then
      echo "ERROR: Cargo ($CARGO_VERSION) != Node ($NODE_VERSION)"
      exit 1
    fi
```

### Post-Build Verification

After sync-versions execution:

1. **File validation**: All target manifests updated
2. **Version match**: Spot-check across ecosystems
3. **Syntax validation**: Each manifest valid YAML/JSON/TOML
4. **Git status**: No unexpected file changes

### Release CI Gate

Version consistency check must:
- Run on every commit to main
- Block release if versions diverge
- Report which ecosystem is out of sync
- Prevent merge of manual version edits to language-specific manifests

## Agent Coordination

The **dependency-management-coordinator** agent is responsible for:

- Approving and executing version synchronization
- Validating sync-versions output before release
- Auditing manual version edits to manifests
- Coordinating with release-versioning-coordinator agent
- Maintaining sync_versions.py script

## Procedures

### Standard Version Bump Workflow

1. **Determine version**: Decide on X.Y.Z bump
2. **Edit Cargo.toml only**: Update workspace.package.version
3. **Run sync**: Execute `task sync-versions`
4. **Review changes**: `git diff` to verify all ecosystems updated
5. **Commit together**: Single commit with all manifest updates
6. **CI verification**: Wait for version consistency checks to pass
7. **Release**: Proceed with multi-platform-release-coordination rule

### Emergency Version Rollback

If version sync fails:

1. **Revert commit**: `git revert` the problematic sync commit
2. **Fix sync script**: Update scripts/sync_versions.py if needed
3. **Re-run manual validation**: Test sync against all manifests
4. **Execute sync again**: Run `task sync-versions` fresh
5. **Report issue**: Document what caused the sync failure

## Validation Checklist

Before release, verify:

- [ ] Cargo.toml workspace.package.version is target version
- [ ] `task sync-versions` ran without errors
- [ ] All manifest files contain identical version
- [ ] No manual edits to language-specific version fields
- [ ] CI version consistency check passes
- [ ] Git diff shows only version field changes
- [ ] No version mismatches across ecosystems
- [ ] Commit message documents version bump

## Never

- Manually edit version in pyproject.toml, package.json, etc. (except Cargo.toml)
- Use `npm version` or similar ecosystem-specific version commands
- Release with version inconsistencies across ecosystems
- Bump versions before running `task sync-versions`
- Create package releases without version sync CI pass
