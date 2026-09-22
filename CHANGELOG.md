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

- updates to documentation

### Fixed

## [0.0.15] - 2026-09-20

### Added

- New `acr-build-push.yml` reusable workflow: language-agnostic, publishes a
  prebuilt app's image to an existing ACR via `publish-acr-image`. Takes
  `app-version` as a required input instead of resolving it from a build.

### Changed

### Fixed

## [0.0.14] - 2026-09-20

### Added

### Changed

- **Breaking:** `acr-build-deploy-java.yml` is renamed to
  `acr-build-push-java.yml`. Consumers must update their `uses:` path.
- **Breaking:** `acr-build-deploy-python.yml` is renamed to
  `acr-build-push-python.yml`. Consumers must update their `uses:` path.

### Fixed

## [0.0.13] - 2026-09-19

### Added

### Changed

### Fixed

- `setup-python-poetry` now passes `version` instead of `poetry-version` to
  `snok/install-poetry@v1`, matching that action's actual input name.

## [0.0.12] - 2026-09-19

### Added

### Changed

- `setup-python-poetry` now installs Poetry via `snok/install-poetry@v1`
  instead of the unmaintained `Gr1N/setup-poetry@v8`.
- Documented in `DEVELOPMENT_WORKFLOW.md` that `release.yml` force-moves the
  major tag, requiring `git fetch --tags --force` to sync it locally.

### Fixed

## [0.0.11] - 2026-09-18

### Added

- New `publish-acr-image` composite action: sign in, verify the registry,
  build/push, verify, smoke-test, and purge an image in an existing ACR.
  Extracted from `acr-build-deploy.yml` so it can be shared by
  language-specific reusable workflows.
- New `acr-build-deploy-python.yml` reusable workflow for Python/Poetry
  projects, built on `setup-python-poetry`, `poetry-build`, and
  `publish-acr-image`.

### Changed

- **Breaking:** `acr-build-deploy.yml` is renamed to
  `acr-build-deploy-java.yml`. Consumers must update their `uses:` path.
- **Breaking:** `acr-build-deploy-java.yml` (formerly `acr-build-deploy.yml`)
  now requires an `artifact-id` input instead of reading `artifactId` from
  `app/gradle.properties`.

### Fixed

## [0.0.10] - 2026-09-17

### Added

### Changed

- **Breaking:** removed the `confirm` input from `acr-repo-delete`. The
  `allowed-actor` safeguard is now the only gate before deletion.

### Fixed

## [0.0.9] - 2026-09-17

### Added

### Changed

- **Breaking:** `acr-repo-delete` now requires a new `artifact-id` input
  instead of reading `artifactId` from the caller's `app/gradle.properties`;
  the workflow no longer checks out the caller's repository.

### Fixed

## [0.0.8] - 2026-09-17

### Added

- New `poetry-build-verify` reusable workflow for Python/Poetry projects:
  runs the static type checker, static code analysis, and unit test
  coverage, with an opt-in SonarCloud quality gate.
- New `setup-python-poetry` and `poetry-build` composite actions, used by
  `poetry-build-verify`.

### Changed

- **Breaking:** `confirm` on `acr-repo-delete` is now a string that must
  exactly match `DELETE REPO <registry-name> <environment>/<artifactId>`,
  replacing the boolean confirmation checkbox.

### Fixed

## [0.0.7] - 2026-09-15

### Added

### Changed

- **Breaking:** `gradle-args`, `properties-file`, and `release-branch` are no
  longer inputs on `gradle-release`; each is fixed at its former default
  value.

### Fixed

## [0.0.6] - 2026-09-15

### Added

### Changed

- **Breaking:** `environment` and `registry-name` are now required inputs
  (no default) on `acr-build-deploy` and `acr-repo-delete`.
- **Breaking:** `runs-on` is no longer an input; every reusable workflow now
  runs on `ubuntu-latest`.
- **Breaking:** `project-path`, `gradle-args`, `artifact-properties-path`,
  `docker-context`, `smoke-test-entrypoint`, `smoke-test-args`, and
  `dotenv-path` are no longer inputs; each is fixed at its former default
  value.

### Fixed

## [0.0.5] - 2026-09-15

### Added

### Changed

### Fixed

- fixed confirm and environment

## [0.0.4] - 2026-09-15

### Added

### Changed

### Fixed

- fixed environment

## [0.0.3] - 2026-09-15

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

