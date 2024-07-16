---
title: "Publish Workflow"
render_macros: false
---
# Publish Workflow
The Publish workflow assembles and publishes the project to the repositories defined in the project, and creates a GitHub release.


## Usage
To use the Publish workflow in your GitHub repository, create a `.github/workflows/publish.yaml` file with at minimum the following contents:

```yaml title=".github/workflows/publish.yaml"
---
name: 'Publish'

on:  # yamllint disable-line rule:truthy
  push:
    tags:
      - "release-*.*.*"

jobs:
  publish:
    uses: metaborg/actions/.github/workflows/gradle-publish.yaml@main
    secrets:
      METABORG_ARTIFACTS_USERNAME: ${{ secrets.METABORG_ARTIFACTS_USERNAME }}
      METABORG_ARTIFACTS_PASSWORD: ${{ secrets.METABORG_ARTIFACTS_PASSWORD }}
```

!!! note ""
    Adjust the pattern of release tags, for example, to match those configured in [Gitonium](https://github.com/metaborg/gitonium).

Here we assume the project is configured to publish to [Metaborg Artifacts](https://artifacts.metaborg.org/) using the [`org.metaborg.convention.maven-publish` plugin](https://github.com/metaborg/metaborg-gradle/). As a result, the repository must have the following secrets defined:

- `METABORG_ARTIFACTS_USERNAME`: The username of the special user that can publish to Metaborg Artifacts.
- `METABORG_ARTIFACTS_PASSWORD`: The password or API key for the user that can publish to Metaborg Artifacts.

!!! note ""
    For all repositories under the [`metaborg` GitHub organization](https://github.com/metaborg/), these secrets have already been configured as _Organization secrets_.


## Advanced Usage
You can adjust several parameters to the workflow:

- `gradle-command`: The Gradle command to invoke to publish the project.
- `gradle-version-command`: The Gradle command to invoke that prints just the project version to stdout.
- `os`: The operating system to use.
- `java-distribution`: The Java distribution to use.
- `java-version`: The Java version to use.
- `gradle-version`: The Gradle version to use.
- `gradle-build-scan-publish`: Whether to publish a build scan.

```yaml title=".github/workflows/publish.yaml"
# ...
jobs:
  publish:
    uses: metaborg/actions/.github/workflows/gradle-publish.yaml@main
    with:
      gradle-command: |
        gradle assemble publish
      gradle-version-command: |
        gradle -q :printVersion
      os: 'ubuntu-latest'
      java-distribution: 'temurin'
      java-version: '11'
      gradle-version: '7.6.4'
      gradle-build-scan-publish: false
```

!!! tip ""
    For the default values, see [the definition](https://github.com/metaborg/actions/blob/main/.github/workflows/gradle-publish.yaml).


## Example
A good example of a project using this workflow is the [Gitonium](https://github.com/metaborg/gitonium) project. See the Publish workflow at [`.github/workflows/dependencies.yaml`](https://github.com/metaborg/gitonium/blob/master/.github/workflows/publish.yaml) and the results of the workflow on the [Actions](https://github.com/metaborg/gitonium/actions/workflows/publish.yaml) page.
