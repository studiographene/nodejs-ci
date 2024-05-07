> This repo has auto Git release workflow using Release-Please. For guide and to know how it works, ref to [Git Auto Release Trunk Tag Based CI/CD](https://studiographene.atlassian.net/wiki/spaces/SGKB/pages/2147615558/Git+Auto+Release+Trunk+Tag+Based+CI+CD)

# Purpose

CI scans workflows for NodeJS based projects.

## How To Use:

### CI workflow

1. Create a file `ci.yml` with below content (change inputs as required).

#### ci.yml

```sh
name: sg-ci

on:
  pull_request: {}
  issue_comment:

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

| Name                     | Description                                                                 | Default                          |
| ------------------------ | --------------------------------------------------------------------------- | -------------------------------- |
| excluded_jobs            | A string of comma separated jobs that you want to exculude.                 |                                  |
| package_manager          |                                                                             | npm                              |
| npm_token                | NPM token                                                                   |                                  |
| build_command            | build command for the project                                               | `npm run build`                  |
| docker_build_command     | Docker build command                                                        | `docker build -t local:latest .` |
| docker_build_image_id    | Docker image ID as mentioned in docker_build_command                        | `local:latest`                   |
| CONTAINER_SCANNERS:      | comma-separated list of what security issues to detect (vuln,secret,config) | `vuln`                           |
| CONTAINER_SCAN_SKIP_DIRS | Comma separated list of directories to skip scanning                        |                                  |
| lint_command             | lint command for the project                                                | `npm run lint`                   |
| allowedLicenses          | A file containing allowed licenses name in License scan finding             |                                  |
| semgrep_options          |                                                                             |                                  |
security_scan_before_step_command    | Optional commands to pass before secuirty scan job |                               |
security_scan_after_step_command    | Optional commands to pass after secuirty scan job steps execution |                               |
caching_before_step_command    | Optional commands to pass before caching job steps execution |                   |
caching_after_step_command    | Optional commands to pass after caching job steps execution |                   |
technology_based_scans_before_step_command    | Optional commands to pass before techology based scans job steps execution |                   |
technology_based_scans_after_step_command    | Optional commands to pass after techology based scans job steps execution |                   |
pr_agent_before_step_command    | Optional commands to pass before Codium PR agent job steps execution |                   |
 pr_agent_after_step_command    | Optional commands to pass after Codium PR agent job steps execution |                   |
---

### Jobs list:
#### Jobs have nested steps which are running the mentioned scans.

- Security scans
  - SAST Scan
  - Gitleaks scan
  - License Scan
  - Dependency Scan using Google OSV
- Technology based scans
  - Eslint scan
  - Docker Build
  - Trivy container vulnerability scan
  - Build project
- Codium PR Agent Scan
- dependencies_report_pulse
- sast_report_pulse
