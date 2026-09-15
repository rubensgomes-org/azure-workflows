# Changelog

All notable changes to this project are documented in this file.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versioning: [Semantic Versioning](https://semver.org/) with **consumer-impact
semantics**: PATCH for documentation and in-place tweaks that no caller can
observe, MINOR for new reusable workflows, new composite actions, or new
optional inputs, MAJOR for anything that breaks a consumer stub — a removed or
renamed input, a changed default, a removed workflow.

Add entries under `[Unreleased]` as you work. Cutting a release renames
`[Unreleased]` to the new version and re-seeds an empty `[Unreleased]` block
above it; see the "Cutting a Release" section in `README.md` for the full
procedure.

`[Unreleased]` is for *changes since the last release only*.

The version headings here are load-bearing: `release.yml` refuses to publish a
tag with no matching `## [X.Y.Z]` section, and the body of that section becomes
the GitHub Release notes.

## [Unreleased]

### Added

### Changed

- `acr-repo-delete` now succeeds when the registry does not exist, matching
  its existing behavior for a repository that does not exist. A failure to
  query Azure still fails the run.
- **Breaking:** the `acr-repo-delete` confirm phrase is now
  `DELETE REPO <registry-name> <environment>/<artifactId>`, so a mistyped
  registry cannot pass the safeguard.

### Fixed

## [0.0.2] - 2026-09-14

### Added

### Changed

### Fixed

- fixed defaults

## [0.0.1] - 2026-09-14

### Added
- Initial release.
### Changed

### Fixed

