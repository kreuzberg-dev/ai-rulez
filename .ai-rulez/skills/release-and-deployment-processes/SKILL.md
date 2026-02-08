______________________________________________________________________

## priority: critical

# Release & Deployment Processes

## Semantic Versioning

MAJOR.MINOR.PATCH. Pre-releases: `-alpha`, `-beta`, `-rc`. Tags: `v1.2.3` (signed when possible). Single source of truth: `Cargo.toml` → sync to all manifests.

## Coordinated Multi-Platform Release

All language bindings released same day from `release/vX.Y.Z` branch.

| Registry | Tool | Command |
|----------|------|---------|
| crates.io | cargo | `cargo publish --token $CARGO_TOKEN` |
| PyPI | twine | `python -m build && twine upload dist/*` |
| npm | npm | `npm publish --access public` |
| RubyGems | gem | `gem build && gem push *.gem` |
| Maven Central | Gradle/Sonatype | `./gradlew publish` (GPG-signed) |
| Go proxy | git | `git tag v1.2.3 && git push --tags` |
| NuGet | dotnet | `dotnet pack && dotnet nuget push` |

## Release Automation

GitHub Actions workflow triggered on `v*` tags. Jobs: publish-rust → publish-python → publish-npm → create-github-release with artifacts.

## Changelog

Use `git-cliff` with `cliff.toml` parsing conventional commits (feat:, fix:, etc.). Run `git cliff --output CHANGELOG.md`.

## Release Checklist

1. All tests passing, security scanning clean
1. Version bump + sync across all manifests
1. Generate changelog
1. Create signed tag, push
1. Publish to all registries
1. Verify installations, update docs, announce

## Anti-Patterns

- Manual registry uploads (always automate)
- Version drift between bindings
- Releasing without full test matrix
- Skipping changelog generation
- Publishing to one registry only
