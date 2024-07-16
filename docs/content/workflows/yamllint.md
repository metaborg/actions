---
title: "Yamllint Workflow"
---
# Yamllint Workflow
The Yamllint workflow checks the validity of YAML files in the repository.


## Usage
To use the Yamllint workflow in your GitHub repository, create a `.github/workflows/liny.yaml` file with at minimum the following contents:

```yaml title=".github/workflows/lint.yaml"
---
name: 'Lint'

on:  # yamllint disable-line rule:truthy
  push:

jobs:
  dependencies:
    uses: metaborg/actions/.github/workflows/yaml-lint.yaml@main
```

This lint can optionally be used with a `.yamllint.yaml` configuration file in the root of the repository. For example:

```yaml
---
extends: default

rules:
  comments-indentation: disable
  line-length:
    max: 120
```
