# CI Workflows and Actions
[![Build][github-build-badge]][github-build]
[![License][license-badge]][license]
[![Documentation][documentation-badge]][documentation]

This repository contains [reusable GitHub workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows) and GitHub actions for Metaborg/Spoofax repositories.

[![Documentation][documentation-button]][documentation]

| Workflow                          | Description                                                                   |
| --------------------------------- | ----------------------------------------------------------------------------- |
| `gradle-build-matrix.yaml`        | Build Java Gradle projects using the default build matrix.                    |
| `gradle-build.yaml`               | Build Java Gradle projects.                                                   |
| `gradle-publish.yaml`             | Signs and publishes the artifact to GitHub and Metaborg Artifacts.            |
| `gradle-dependencies.yaml`        | Submits the dependencies to GitHub.                                           |
| `mkdocs-material.yaml`            | Builds and deploys the project's MkDocs Material documentation.               |
| `yaml-lint.yaml`                  | Lints the YAML files in the repository.                                       |



## License
Copyright 2024 Delft University of Technology

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at <https://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an **"as is" basis, without warranties or conditions of any kind**, either express or implied. See the License for the specific language governing permissions and limitations under the License.



[github-build-badge]: https://img.shields.io/github/actions/workflow/status/metaborg/actions/yaml-lint.yaml
[github-build]: https://github.com/metaborg/actions/actions
[license-badge]: https://img.shields.io/github/license/metaborg/actions
[license]: https://github.com/metaborg/actions/blob/main/LICENSE
[documentation-badge]: https://img.shields.io/badge/docs-latest-brightgreen
[documentation]: https://spoofax.dev/actions/
[documentation-button]: https://img.shields.io/badge/Documentation-blue?style=for-the-badge&logo=googledocs&logoColor=white
