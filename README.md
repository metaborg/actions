# Workflows
This repository contains [reusable GitHub workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) for Metaborg/Spoofax repositories.



## Gradle Dependencies
The `gradle-dependencies.yaml` workflow submits the dependencies of the project to GitHub. To apply the workflow:

```yaml
---
name: 'Submit Dependencies'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main

jobs:
  build:
    uses: metaborg/workflows/.github/workflows/gradle-dependencies.yaml@v1
```


## MkDocs Material
The `mkdocs-material.yaml` workflow sets up Python and MkDocs Material and builds and deploys the project's documentation. To apply the workflow:

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
    uses: metaborg/workflows/.github/workflows/mkdocs-material.yaml@v1
```


## Yaml Lint
The `yamllint.yaml` workflow lints the YAML files in the repository according to the `.yamllint.yaml` file in the repository's root. To apply the workflow:

```yaml
---
name: 'Lint'

on:  # yamllint disable-line rule:truthy
  push:
    branches:
      - main

jobs:
  lint:
    uses: metaborg/workflows/.github/workflows/yaml-lint.yaml@v1
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
