[![AI Assisted](https://img.shields.io/badge/AI--Assisted-Development-007ACC?logo=openai&logoColor=white)](./AI_DISCLAIMER.md)

# azure-workflows

This project contains a collection of reusable GitHub Actions workflows and
composite actions used across Azure Java and Spring Boot projects maintained by
[Rubens Gomes](https://rubensgomes.com/).

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
- An Azure RBAC role that allows you to create the resources, such as resource
  groups, container registry, container apps, and databases.
- GitHub account
- UNIX-based operating system (for example, AIX, Linux, macOS, or Solaris)
- Azure CLI 2.90+
- Terraform 1.16.0+
- GitHub CLI (`gh`) 2.99+
- Git 2.55+
- GNU Make 3.8+

### Configuration

Follow the steps in the [INITIAL_SETUP](./docs/INITIAL_SETUP.md).

## GitHub Actions

| Reusable workflow             | Purpose                                                                                     |
|--------------------------------|----------------------------------------------------------------------------------------------|
| `gradle-build-verify.yml`      | compile, test, check, assemble, then block on the SonarCloud quality gate                   |
| `gradle-release.yml`           | `./gradlew release` (net.researchgate.release). **Writes to the repository**                |
| `poetry-build-verify.yml`      | install, then mypy, pylint, pytest, then block on the SonarCloud quality gate               |
| `acr-build-push-java.yml`      | build a Java/Gradle app, publish its image to an existing ACR, verify, smoke-test, purge    |
| `acr-build-push-python.yml`    | build a Python/Poetry app, publish its image to an existing ACR, verify, smoke-test, purge  |
| `acr-repo-delete.yml`          | **destructive** — delete a repository and all its tags from an ACR                          |

| Composite action       | Purpose                                                     |
|--------------------------|---------------------------------------------------------------|
| `setup-java-gradle`    | install the pinned JDK, configure Gradle                     |
| `gradle-build`         | compile / test / check / assemble as four red-green steps    |
| `setup-python-poetry`  | install the pinned Python, configure Poetry                  |
| `poetry-build`         | mypy / pylint / pytest as three red-green steps               |
| `azure-login`          | `az login` as a service principal, select the subscription   |
| `verify-acr-registry`  | assert a registry exists, return its login server             |
| `publish-acr-image`    | build, push, verify, smoke-test, and purge an image in an ACR |

| Repository workflow | Purpose                                                                                                     |
|---------------------|-------------------------------------------------------------------------------------------------------------|
| `lint.yml`          | self-CI on push and pull request — `actionlint`, plus internal `uses:` refs agree                           |
| `release.yml`       | fires on a `v*.*.*` tag push — validate tag against `CHANGELOG.md`, move the major tag, publish the release |

## Development Workflow

See [DEVELOPMENT_WORKFLOW](./docs/DEVELOPMENT_WORKFLOW.md) for guidance on 
developing, and cutting a release on this project.

---
Author:  [Rubens Gomes](https://rubensgomes.com/)
