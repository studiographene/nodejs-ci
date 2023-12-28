# nodejs-ci

CI scans workflows for NodeJS based projects. Following is the sample code of the CI pipeline

## How To Use:

### CI workflow

1. Create a file `ci.yml` with below content (change inputs as required).

#### ci.yml

```sh
name: sg-ci

on:
  pull_request: {}

jobs:
  ci:
    uses: studiographene/nodejs-ci/.github/workflows/ci.yml@master
    with:
      package_manager: pnpm
      build_command: pnpm run build
      lint_command: pnpm run lint
      excluded_jobs: docker
    secrets: inherit
    permissions: write-all
```

### Pulse Dependencies Analytics workflow

1. Create a file `dependencies-analytics.yml` with below content (change inputs as required).

#### dependencies-analytics.yml

```sh
name: sg-analytics

on:
  push: [<enter-branch-name>]

jobs:
  analytics:
    uses: studiographene/nodejs-ci/.github/workflows/dependencies-analytics.yml@master
    secrets: inherit
    permissions: write-all
```

### Pulse SAST Analytics workflow

1. Create a file `sast-analytics.yml` with below content (change inputs as required).

#### sast-analytics.yml

```sh
name: sg-analytics

on:
  push: [<enter-branch-name>]

jobs:
  analytics:
    uses: studiographene/nodejs-ci/.github/workflows/sast-analytics.yml@master
    secrets: inherit
    permissions: write-all
```

---

## Inputs

### Required:

| Name | Description |
| ---- | ----------- |
|      |             |

### Optional:

| Name                 | Description                                                     | Default                          |
| -------------------- | --------------------------------------------------------------- | -------------------------------- |
| excluded_jobs        | A string of comma separated jobs that you want to exculude.     |                                  |
| package_manager      |                                                                 | npm                              |
| npm_token            | NPM token                                                       |                                  |
| build_command        | build command for the project                                   | `npm run build`                  |
| docker_build_command | Docker build command                                            | `docker build -t local:latest .` |
| lint_command         | lint command for the project                                    | `npm run lintß`                  |
| allowedLicenses      | A file containing allowed licenses name in License scan finding |                                  |
| semgrep_options      |                                                                 |                                  |

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
