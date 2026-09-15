# Initial Setup

This file contains instructions to be followed prior to running the software of
this project.

## Action Secrets

Callers map secrets **explicitly**. `secrets: inherit` only forwards secrets
whose names match exactly, so it would not map `RUBENS_PAT_TOKEN` onto the
declared `packages-token`.

| Declared name         | Typically mapped from | Required by                                                 | Notes                                                                        |
|-----------------------|-----------------------|-------------------------------------------------------------|------------------------------------------------------------------------------|
| `packages-token`      | `RUBENS_PAT_TOKEN`    | `gradle-build-verify`, `gradle-release`, `acr-build-deploy` | classic PAT, `read:packages`. `gradle-release` also needs `repo` — it pushes |
| `sonar-token`         | `SONAR_TOKEN`         | `gradle-build-verify` (only when `run-sonar: true`)         | must be able to **read quality gate status**, not just submit analyses       |
| `azure-client-secret` | `AZURE_CLIENT_SECRET` | `acr-build-deploy`, `acr-repo-delete`                       | service principal client secret                                              |

Every one of these is declared `required: true` except `sonar-token`, and an
unset secret arrives as the **empty string** rather than as an error. That is
why the consumers of these values check for emptiness themselves: `azure-login`
validates all four Azure values in one pass so a single run reports every
missing one, and the `sonar` step in `gradle-build-verify.yml` fails outright if
`run-sonar` was set to `true` without a token.

## Action Variables

| Declared name           | Typically mapped from   | Required by                           | Notes                                                                          |
|-------------------------|-------------------------|---------------------------------------|--------------------------------------------------------------------------------|
| `azure-client-id`       | `AZURE_CLIENT_ID`       | `acr-build-deploy`, `acr-repo-delete` | service principal application (client) ID                                      |
| `azure-tenant-id`       | `AZURE_TENANT_ID`       | `acr-build-deploy`, `acr-repo-delete` | Entra ID tenant the principal belongs to                                       |
| `azure-subscription-id` | `AZURE_SUBSCRIPTION_ID` | `acr-build-deploy`, `acr-repo-delete` | subscription holding the registry — four separate values, not one `creds` JSON |

Callers pass these under `with:`, not `secrets:`. They identify the principal
but grant nothing without `azure-client-secret`, so keeping them variables
leaves them readable in the log of a failed sign-in instead of masked as `***`.

## Consumer stubs

### build-verify

```yaml
name: build-verify
on:
  workflow_dispatch:
permissions:
  contents: read
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: false
jobs:
  build-verify:
    uses: rubensgomes-org/azure-workflows/.github/workflows/gradle-build-verify.yml@v0
    # The SonarCloud quality gate is opt-in. Drop this "with:" block to build
    # and verify without it; the sonar step then shows as skipped.
    with:
      run-sonar: true
    secrets:
      packages-token: ${{ secrets.RUBENS_PAT_TOKEN }}
      sonar-token: ${{ secrets.SONAR_TOKEN }}
```

SpotBugs needs no input here. The consumer build applies the
`com.github.spotbugs` Gradle plugin, which attaches `spotbugsMain` to the
`check` lifecycle task, so it runs inside the build step and a finding fails the
job.

### release

```yaml
name: release
on:
  workflow_dispatch:
permissions:
  contents: write
concurrency:
  group: release
  cancel-in-progress: false
jobs:
  release:
    uses: rubensgomes-org/azure-workflows/.github/workflows/gradle-release.yml@v0
    secrets:
      packages-token: ${{ secrets.RUBENS_PAT_TOKEN }}
```

### acr-build-deploy

```yaml
name: acr-build-deploy
on:
  workflow_dispatch:
    inputs:
      environment:
        description: >-
          Image namespace, used as <environment>/<artifactId>:<tag>.
        required: false
        default: lab
        type: string
      tag:
        description: >-
          Image tag. Empty defaults to the application version (APP_VERSION in
          .env).
        required: false
        type: string
      registry_name:
        description: >-
          Name of the EXISTING Azure Container Registry to push to (not the
          login server).
        required: false
        #default: crrgomeslab01
        default: crrgomeslab01
        type: string
permissions:
  contents: read
concurrency:
  group: acr-build-deploy
  cancel-in-progress: false
jobs:
  build:
    uses: rubensgomes-org/azure-workflows/.github/workflows/acr-build-deploy.yml@v0
    with:
      environment: ${{ inputs.environment }}
      tag: ${{ inputs.tag }}
      registry-name: ${{ inputs.registry_name }}
      azure-client-id: ${{ vars.AZURE_CLIENT_ID }}
      azure-tenant-id: ${{ vars.AZURE_TENANT_ID }}
      azure-subscription-id: ${{ vars.AZURE_SUBSCRIPTION_ID }}
    secrets:
      packages-token: ${{ secrets.RUBENS_PAT_TOKEN }}
      azure-client-secret: ${{ secrets.AZURE_CLIENT_SECRET }}
```

### acr-repo-delete

The `description:` text is what the operator reads before typing the
confirmation, so it is reproduced in full in the stub rather than summarised.

```yaml
name: acr-repo-delete
on:
  workflow_dispatch:
    inputs:
      environment:
        description: >-
          Image namespace. With artifactId this forms the repository
          <environment>/<artifactId>.
        required: false
        default: lab
        type: string
      registry_name:
        description: >-
          Name of the EXISTING Azure Container Registry to delete from (not the
          login server).
        required: false
        default: crrgomeslab01
        type: string
      confirm:
        description: >
          EVERY TAG AND MANIFEST IN THE REPOSITORY IS DELETED PERMANENTLY.
          There is no soft-delete and no recycle bin. Push anything you care
          about elsewhere first.
          SAFEGUARD: type EXACTLY "DELETE REPO <environment>/<artifactId>" --
          e.g. DELETE REPO lab/azure-acr
        required: true
        type: string
permissions:
  contents: read
concurrency:
  group: acr-repo-delete
  cancel-in-progress: false
jobs:
  delete:
    uses: rubensgomes-org/azure-workflows/.github/workflows/acr-repo-delete.yml@v0
    with:
      environment: ${{ inputs.environment }}
      registry-name: ${{ inputs.registry_name }}
      confirm: ${{ inputs.confirm }}
      azure-client-id: ${{ vars.AZURE_CLIENT_ID }}
      azure-tenant-id: ${{ vars.AZURE_TENANT_ID }}
      azure-subscription-id: ${{ vars.AZURE_SUBSCRIPTION_ID }}
    secrets:
      azure-client-secret: ${{ secrets.AZURE_CLIENT_SECRET }}
```

## Assumptions about consumers

Consumer projects must match the layout of
[spring-blueprint](https://github.com/rubensgomes-org/spring-blueprint)

| Assumption                                                    | Input                               |
|---------------------------------------------------------------|-------------------------------------|
| Gradle subproject is `:app`                                   | `project-path`                      |
| Toolchain is Microsoft JDK 25                                 | `java-version`, `java-distribution` |
| `artifactId` is in `app/gradle.properties`                    | `artifact-properties-path`          |
| `developerName` / `developerEmail` are in `gradle.properties` | `properties-file`                   |

---
Author:  [Rubens Gomes](https://rubensgomes.com/)
