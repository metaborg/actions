---
title: "Documentation Workflow"
---
# Documentation Workflow
The Documentation workflow builds the MkDocs documentation of a project and publishes it to GitHub Pages.

!!! tip "Documentation Setup"
    To quickly setup documentation in your project, copy the `docs/` folder from the [mkdocs-material-template](https://github.com/Virtlink/mkdocs-material-template) project into the `/docs/` directory in the root of your repository.


## Usage
To use the Documentation workflow in your GitHub repository, create a `.github/workflows/documentation.yaml` file with at minimum the following contents:

```yaml title=".github/workflows/documentation.yaml"
---
name: 'Documentation'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main
  workflow_dispatch: {} # Allow running this workflow manually (Actions tab)

jobs:
  documentation:
    uses: metaborg/actions/.github/workflows/mkdocs-material.yaml@main
```

!!! note ""
    Adjust the name of the branches whose dependencies are submitted on push.


## Advanced Usage
You can adjust several parameters to the workflow:

- `docs-dir`: The subdirectory with the MkDocs documentation.

```yaml title=".github/workflows/documentation.yaml"
# ...
jobs:
  documentation:
    uses: metaborg/actions/.github/workflows/mkdocs-material.yaml@main
    with:
      docs-dir: 'docs'
```

!!! tip ""
    For the default values, see [the definition](https://github.com/metaborg/actions/blob/main/.github/workflows/mkdocs-material.yaml).


## Example
A good example of a project using this workflow is the [Gitonium](https://github.com/metaborg/gitonium) project. See the Documentation workflow at [`.github/workflows/documentation.yaml`](https://github.com/metaborg/gitonium/blob/master/.github/workflows/documentation.yaml) and the results of the workflow on the [Actions](https://github.com/metaborg/gitonium/actions/workflows/documentation.yaml) page.
