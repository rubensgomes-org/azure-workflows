# Reusable GitHub Workflows

[![GitHub](https://img.shields.io/badge/GitHub-Actions-0969da?logo=github+actions)](https://github.com/features/actions)
[![Microsoft](https://img.shields.io/badge/Microsoft-Azure-0969da)](https://azure.microsoft.com/en-us)
[![AI](https://img.shields.io/badge/AI-Assisted-d29922?logo=claude+code)](https://github.com/rubensgomes-org/azure-workflows/blob/main/AI_DISCLAIMER.md)
[![license](https://img.shields.io/badge/license-MIT-1a7f37)](https://github.com/rubensgomes-org/azure-workflows/blob/main/LICENSE)

This project contains a collection of reusable GitHub Actions workflows and
composite actions used across Python, Java/Spring Boot, and Microsoft Azure
projects maintained by
[Rubens Gomes](https://rubensgomes.com/).

---

## AI Disclaimer

This project includes code and documentation created with the assistance of AI
tools. For details on usage, limits, and review practices, please see
the [AI_DISCLAIMER](./AI_DISCLAIMER.md).

## Installation

To use this project, ensure that your environment is properly configured and
that the required tools are installed.

### Prerequisites

The following prerequisites are required:

- Microsoft Azure account
- An active Azure subscription
- An Azure RBAC role that allows you to create resources such as resource
  groups, container registries, container apps, and databases.
- GitHub account
- UNIX-based operating system (for example, AIX, Linux, macOS, or Solaris)
- Azure CLI 2.90+
- Terraform 1.16.0+
- GitHub CLI (`gh`) 2.99+
- Git 2.55+
- GNU Make 3.8+

### Configuration

Follow the steps in [INITIAL_SETUP](./docs/INITIAL_SETUP.md).

## GitHub Actions

| Reusable workflow           | Purpose                                                                                          |
|-----------------------------|--------------------------------------------------------------------------------------------------|
| `gradle-build-verify.yml`   | compile, test, check, assemble, then block on the SonarCloud quality gate                        |
| `gradle-release.yml`        | `./gradlew release` (net.researchgate.release). **Writes to the repository**                     |
| `poetry-build-verify.yml`   | install, then mypy, pylint, pip-audit, pytest, then block on the SonarCloud quality gate         |
| `poetry-publish-pypi.yml`   | rename `[Unreleased]` in `CHANGELOG.md`, build, publish to PyPI. **Writes to the repository**    |
| `acr-build-push-java.yml`   | build a Java/Gradle app, publish its image to an existing ACR, verify, smoke-test, purge         |
| `acr-build-push-python.yml` | build a Python/Poetry app, publish its image to an existing ACR, verify, smoke-test, purge       |
| `acr-build-push.yml`        | language-agnostic — publish a prebuilt app's image to an existing ACR, verify, smoke-test, purge |
| `acr-repo-delete.yml`       | **destructive** — delete a repository and all its tags from an ACR                               |

| Composite action      | Purpose                                                       |
|-----------------------|---------------------------------------------------------------|
| `setup-java-gradle`   | install the pinned JDK, configure Gradle                      |
| `gradle-build`        | compile / test / check / assemble as four red-green steps     |
| `setup-python-poetry` | install the pinned Python, configure Poetry                   |
| `poetry-build`        | mypy / pylint / pip-audit / pytest as four red-green steps    |
| `azure-login`         | `az login` as a service principal, select the subscription    |
| `verify-acr-registry` | assert a registry exists, return its login server             |
| `publish-acr-image`   | build, push, verify, smoke-test, and purge an image in an ACR |

| Repository workflow | Purpose                                                                                                     |
|---------------------|-------------------------------------------------------------------------------------------------------------|
| `lint.yml`          | self-CI on push and pull request — `actionlint`, plus a check that internal `uses:` refs agree              |
| `release.yml`       | fires on a `v*.*.*` tag push — validate tag against `CHANGELOG.md`, move the major tag, publish the release |

## Development Workflow

See [DEVELOPMENT_WORKFLOW](./docs/DEVELOPMENT_WORKFLOW.md) for guidance on
developing and cutting a release for this project.

## Open Source Project Information

This project is open source and publicly hosted on GitHub at
[Math AI Agent](https://github.com/rubensgomes-org/). It is
published under an
[OSI-approved open-source license](https://opensource.org/licenses).

> **Note:** Public availability and the use of an OSI-approved license are
> requirements for eligibility to use
> https://sonarcloud.io/login under its free plan for
> open-source projects.

## License

This project is licensed under the
[MIT License](https://github.com/rubensgomes-org/azure-workflows/blob/main/LICENSE).

## Links

- [GitHub Project](https://github.com/rubensgomes-org/azure-workflows)
- [Development Workflow](https://github.com/rubensgomes-org/azure-workflows/blob/main/docs/DEVELOPMENT_WORKFLOW.md)
- [Initial Setup](https://github.com/rubensgomes-org/azure-workflows/blob/main/docs/INITIAL_SETUP.md)

---
Author: [Rubens Gomes](https://rubensgomes.com/)
