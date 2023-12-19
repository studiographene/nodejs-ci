# nodejs-ci

CI scans workflows for NodeJS based projects. Following is the sample code of the CI pipeline

```sh
name: studiographene-ci

on:
  pull_request: {}
  workflow_dispatch:

jobs:
  call-workflow:
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

There are a few parametes that can be set as custom inputs (in the with section):

- project_name:  name of the project
- excluded_jobs: A string of jobs that you want to exculude. For multiple, send the jobs comma seperated.
- package_manager: default is npm
- npm_token: NPM token if valid
- build_command: build command for the project
- docker_build_command: Docker build command
- lint_command: lint command for the project
- run_pr_agent: yes|no Defaut value is no

All jobs that are running:

- sast
- osv
- gitleaks
- lint
- build
- docker
- danger
- pr_agent
