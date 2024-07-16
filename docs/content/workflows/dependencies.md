---
title: "Submit Dependencies Workflow"
---
# Submit Dependencies Workflow
The Submit Dependencies workflow gathers the dependencies used in the project and publishes them to GitHub. This enables, for example, the Dependabot Alerts.


## Usage
To use the Submit Dependencies workflow in your GitHub repository, create a `.github/workflows/dependencies.yaml` file with at minimum the following contents:

```yaml title=".github/workflows/dependencies.yaml"
---
name: 'Submit Dependencies'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main

jobs:
  dependencies:
    uses: metaborg/actions/.github/workflows/gradle-dependencies.yaml@main
```

!!! note ""
    Adjust the name of the branches whose dependencies are submitted on push.


## Advanced Usage
You can adjust several parameters to the workflow:

- `os`: The operating system to use.
- `java-distribution`: The Java distribution to use.
- `java-version`: The Java version to use.
- `gradle-version`: The Gradle version to use.
- `gradle-build-scan-publish`: Whether to publish a build scan.

```yaml title=".github/workflows/dependencies.yaml"
# ...
jobs:
  dependencies:
    uses: metaborg/actions/.github/workflows/gradle-dependencies.yaml@main
    with:
      os: 'ubuntu-latest'
      java-distribution: 'temurin'
      java-version: '11'
      gradle-version: '7.6.4'
      gradle-build-scan-publish: true
```

!!! tip ""
    For the default values, see [the definition](https://github.com/metaborg/actions/blob/main/.github/workflows/gradle-dependencies.yaml).


## Example
A good example of a project using this workflow is the [Gitonium](https://github.com/metaborg/gitonium) project. See the Submit Dependencies workflow at [`.github/workflows/dependencies.yaml`](https://github.com/metaborg/gitonium/blob/master/.github/workflows/dependencies.yaml) and the results of the workflow on the [Actions](https://github.com/metaborg/gitonium/actions/workflows/dependencies.yaml) page.
