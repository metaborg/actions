---
title: "Build Workflow"
render_macros: false
---
# Build Workflow
The Build workflow builds your Spoofax Gradle project using the default build matrix of operating systems, Java versions, and Gradle versions. The workflow automatically publishes a build scan.


## Usage
To use the Build workflow in your GitHub repository, create a `.github/workflows/build.yaml` file with at minimum the following contents:

```yaml title=".github/workflows/build.yaml"
---
name: 'Build'

on:  # yamllint disable-line rule:truthy
  push:
  pull_request:
    branches:
      - main

jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-build-matrix.yaml@main
```

!!! note ""
    Adjust the name of the branches whose pull requests are built.


## Advanced Usage
You can adjust several parameters to the workflow:

- `gradle-command`: The Gradle command to invoke to build the project.
- `gradle-build-scan-publish`: Whether to publish a build scan.
- `java-distribution`: The Java distribution to use.

```yaml title=".github/workflows/build.yaml"
# ...
jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-build-matrix.yaml@main
    with:
      gradle-command: |
          gradle build
      gradle-build-scan-publish: true
      java-distribution: 'temurin'
```

!!! tip ""
    For the default values, see [the definition](https://github.com/metaborg/actions/blob/main/.github/workflows/gradle-build-matrix.yaml).


## Publish Snapshots
The [Gitonium](https://github.com/metaborg/gitonium) plugin derives the version of the project from the tags on the Git repository. If the current commit is not tagged, it is considered a _snapshot_ version. This fact can be used to automatically publish snapshot artifacts on each successful build by combining the Build workflow with the [Publish workflow](./publish-workflow.md) into a single workflow, as follows:

```yaml title=".github/workflows/build.yaml"
---
name: 'Build'

on:  # yamllint disable-line rule:truthy
  push:
  pull_request:
    branches:
      - main

jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-build-matrix.yaml@main
  publish:
    if: "github.event_name == 'push' && github.ref == 'refs/heads/main'"
    needs: [build]
    uses: metaborg/actions/.github/workflows/gradle-publish.yaml@main
    with:
      gradle-command: |
          gradle assemble publish -Pgitonium.isSnapshot=true
      gradle-version-command: |
          gradle -q :printVersion -Pgitonium.isSnapshot=true
    secrets:
      METABORG_ARTIFACTS_USERNAME: ${{ secrets.METABORG_ARTIFACTS_USERNAME }}
      METABORG_ARTIFACTS_PASSWORD: ${{ secrets.METABORG_ARTIFACTS_PASSWORD }}
```

This will assemble and publish the build only when new commits were pushed to the main branch and the `build` job succeeded. Note that `gitonium.isSnapshot` is a special Gradle property recognized by the Gitonium plugin that causes a snapshot version to be published even if the current commit is a tagged release.


## Changing the Build Matrix
To change the build matrix for a specific project, using the `gradle-build.yaml` workflow instead. For example:

```yaml title=".github/workflows/build.yaml"
---
name: 'Build'

on:  # yamllint disable-line rule:truthy
  push:
  pull_request:
    branches:
      - main

jobs:
  build:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        java-version: [11]
        gradle-version: ["wrapper", "7.6.4", "8.6", "current"]
    uses: metaborg/actions/.github/workflows/gradle-build.yaml@main
    with:
      os: ${{ matrix.os }}
      java-version: ${{ matrix.java-version }}
      gradle-version: ${{ matrix.gradle-version }}
```


## Example
A good example of a project using this workflow is the [Gitonium](https://github.com/metaborg/gitonium) project. See the Build workflow at [`.github/workflows/build.yaml`](https://github.com/metaborg/gitonium/blob/master/.github/workflows/build.yaml) and the results of the workflow on the [Actions](https://github.com/metaborg/gitonium/actions/workflows/build.yaml) page.
