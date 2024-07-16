---
title: "Build and Publish"
render_macros: false
---
# Build and Publish
The [Gitonium](https://github.com/metaborg/gitonium) plugin derives the version of the project from the tags on the Git repository. If the current commit is not tagged, it is considered a _snapshot_ version. This fact can be used to automatically publish snapshot artifacts on each successful build by combining the [Build workflow](./build.md) with the [Publish workflow](./publish-workflow.md) into a single workflow. This first builds the project, and once this succeeds, publishes the result as either a release (if the build was triggered by pushing a release tag) or a snaphot (otherwise). This can be achieved using the following workflow:

```yaml
---
name: 'Build & Publish'

on:  # yamllint disable-line rule:truthy
  push:
  pull_request:
    branches:
      - main

jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-build-matrix.yaml@main
  publish-snapshot:
    uses: metaborg/actions/.github/workflows/gradle-publish.yaml@main
    with:
      gradle-command: |
        gradle assemble publish -Pgitonium.isSnapshot=true
      gradle-version-command: |
        gradle -q :printVersion -Pgitonium.isSnapshot=true
    if: "github.event_name == 'push' && github.ref == 'refs/heads/main'"
    needs: [build]
    secrets:
      METABORG_ARTIFACTS_USERNAME: ${{ secrets.METABORG_ARTIFACTS_USERNAME }}
      METABORG_ARTIFACTS_PASSWORD: ${{ secrets.METABORG_ARTIFACTS_PASSWORD }}
  publish-release:
    uses: metaborg/actions/.github/workflows/gradle-publish.yaml@main
    if: "github.event_name == 'push' && startsWith(github.ref, 'refs/tags/release-')"
    needs: [build]
    secrets:
      METABORG_ARTIFACTS_USERNAME: ${{ secrets.METABORG_ARTIFACTS_USERNAME }}
      METABORG_ARTIFACTS_PASSWORD: ${{ secrets.METABORG_ARTIFACTS_PASSWORD }}
```

This will assemble and publish the build only when new commits were pushed to the main branch and the `build` job succeeded. Note that `gitonium.isSnapshot` is a special Gradle property recognized by the Gitonium plugin that causes a snapshot version to be published even if the current commit is a tagged release.


## Example
A good example of a project using this workflow is the [Gitonium](https://github.com/metaborg/gitonium) project. See the Build & Publish workflow at [`.github/workflows/build.yaml`](https://github.com/metaborg/gitonium/blob/master/.github/workflows/build.yaml) and the results of the workflow on the [Actions](https://github.com/metaborg/gitonium/actions/workflows/build.yaml) page.
