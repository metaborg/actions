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

