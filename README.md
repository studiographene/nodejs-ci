# nodejs-ci

CI scans workflows for NodeJS based projects. Following is the sample code of the CI pipeline

## How To Use:

Create 2 workflow files under directory `.github/workflows` with below content:

- ci.yml
- analytics.yml

#### ci.yml

```sh
name: studiographene-ci

on:
  pull_request: {}

jobs:
  ci:
    uses: studiographene/nodejs-ci/.github/workflows/ci.yml@master
    with:
      project_name: microservice-boilerplate
      package_manager: pnpm
      build_command: pnpm run build
      lint_command: pnpm run lint
      excluded_jobs: docker
    secrets: inherit
    permissions: write-all
```

#### analytics.yml

```sh
name: sg-analytics

on:
  push: {}

jobs:
  ci:
    uses: studiographene/nodejs-ci/.github/workflows/analytics.yml@master
    secrets: inherit
    permissions: write-all
```

---

## Inputs

### Required:

| Name         | Description              |
| ------------ | ------------------------ |
| project_name | The project/product name |

### Optional:

| Name                 | Description                                                                              | Default                          |
| -------------------- | ---------------------------------------------------------------------------------------- | -------------------------------- |
| excluded_jobs        | A string of jobs that you want to exculude. For multiple, send the jobs comma seperated. |                                  |
| package_manager      |                                                                                          | npm                              |
| npm_token            | NPM token                                                                                |                                  |
| build_command        | build command for the project                                                            | `npm run build``                 |
| docker_build_command | Docker build command                                                                     | `docker build -t local:latest .` |
| lint_command         | lint command for the project                                                             | `npm run lintß`                  |
| allowedLicenses      | A file containing allowed licenses name in License scan finding                          |                                  |
| semgrep_options      |                                                                                          |                                  |
| SAST_OVERRIDE_BRANCH | Non default branch name to run SAST on. If not specified, SAST runs on default branch.   |                                  |

---

### Jobs list:

- sast
- osv
- gitleaks
- lint
- build
- docker
- danger
- pr_agent
- dependencies_report_pulse
- sast_report_pulse
