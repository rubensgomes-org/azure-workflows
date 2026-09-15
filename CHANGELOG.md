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
- Initial release.
### Changed

### Fixed

