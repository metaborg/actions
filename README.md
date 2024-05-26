# CI Workflows and Actions
This repository contains [reusable GitHub workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) and GitHub actions for Metaborg/Spoofax repositories.


## Reusable Workflows
This repository contains reusable GitHub workflows. The following reusable workflows are available:


### Gradle Build
The `gradle-build-matrix.yaml` workflow sets up Java and Gradle and builds the project on the default Matrix of Java and Gradle versions and OS'ses. This shows how to apply the workflow and the default configuration options:

```yaml
---
name: 'Build'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main
    tags:
      - 'release-*.*.*'
  pull_request:
    branches:
      - main

jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-build-matrix.yaml@main
    with:
      os: 'ubuntu-latest'
      java-distribution: 'temurin'
      gradle-build-scan-publish: true
      gradle-command: |
        gradle build
```

> [!NOTE]
> To change the build matrix for a specific project, using the `gradle-build.yaml` workflow instead:
> 
> ```yaml
> ---
> name: 'Build'
> 
> on:  # yamllint disable-line rule:truthy
>   push:
>     branches:
>       - main
>     tags:
>       - 'release-*.*.*'
>   pull_request:
>     branches:
>       - main
> 
> jobs:
>   build:
>     strategy:
>       matrix:
>         os: [ubuntu-latest, windows-latest, macos-latest]
>         java-version: [11]
>         gradle-version: ["wrapper", "7.6.4", "8.6", "current"]
>     uses: metaborg/actions/.github/workflows/gradle-build.yaml@main
>     with:
>       os: ${{ matrix.os }}
>       java-version: ${{ matrix.java-version }}        # default: '11'
>       java-distribution: 'temurin'
>       gradle-version: ${{ matrix.gradle-version }}    # default: 'wrapper'
>       gradle-build-scan-publish: true
>       gradle-command: |
>         gradle build
> ```


### Gradle Publish
The `gradle-publish.yaml` workflow builds, signs, and publishes the artifacts of the project to GitHub and Metaborg Artifacts.

```yaml
---
name: 'Publish'

on:  # yamllint disable-line rule:truthy
  push:
    tags:
      - "release-*.*.*"

jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-publish.yaml@main
    with:
      os: 'ubuntu-latest'
      java-version: '11'
      java-distribution: 'temurin'
      gradle-version: 'wrapper'
      gradle-build-scan-publish: false
      gradle-command: |
        gradle assemble publish
    secrets:
      METABORG_USERNAME: ${{ secrets.METABORG_USERNAME }}
      METABORG_PASSWORD: ${{ secrets.METABORG_PASSWORD }}
      SIGNING_KEY_ID: ${{ secrets.SIGNING_KEY_ID }}
      SIGNING_KEY_PASSWORD: ${{ secrets.SIGNING_KEY_PASSWORD }}
      SIGNING_KEY: ${{ secrets.SIGNING_KEY }}
```





### Gradle Dependencies
The `gradle-dependencies.yaml` workflow submits the dependencies of the project to GitHub. This shows how to apply the workflow and the default configuration options:

```yaml
---
name: 'Submit Dependencies'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main

jobs:
  build:
    uses: metaborg/actions/.github/workflows/gradle-dependencies.yaml@main
    with:
      os: 'ubuntu-latest'
      java-version: '11'
      java-distribution: 'temurin'
      gradle-version: 'wrapper'
      gradle-build-scan-publish: true
      gradle-command: |
        gradle build
```


### MkDocs Material
The `mkdocs-material.yaml` workflow sets up Python and MkDocs Material and builds and deploys the project's documentation. This shows how to apply the workflow and the default configuration options:

```yaml
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
    with:
      docs-dir: 'docs'
```


### Yaml Lint
The `yamllint.yaml` workflow lints the YAML files in the repository according to the `.yamllint.yaml` file in the repository's root. This shows how to apply the workflow and the default configuration options:

```yaml
---
name: 'Lint'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main

jobs:
  lint:
    uses: metaborg/actions/.github/workflows/yaml-lint.yaml@main
```

For example, use the following `.yamllint.yaml` file in the repository root:

```yaml
---
extends: default

rules:
  comments-indentation: disable
  line-length:
    max: 120
```



## License
Copyright 2024 Delft University of Technology

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at <https://www.apache.org/licenses/LICENSE-2.0>.

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an _"as is" basis, without warranties or conditions of any kind_, either express or implied. See the License for the specific language governing permissions and limitations under the License.
