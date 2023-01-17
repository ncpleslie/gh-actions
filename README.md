# gh-actions

Basic notes on GitHub Actions

## Runs on - Hosted runners

<https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners>

Runner image YAML workflow label Notes

Windows Server 2022 windows-latest or windows-2022 The windows-latest label currently uses the Windows Server 2022 runner image.

Windows Server 2019 windows-2019

Ubuntu 22.04 ubuntu-latest or ubuntu-22.04 The ubuntu-latest label currently uses the Ubuntu 22.04 runner image.

Ubuntu 20.04 ubuntu-20.04

Ubuntu 18.04 [deprecated] ubuntu-18.04 Migrate to ubuntu-20.04 or ubuntu-22.04. For more information, see this GitHub blog post.

macOS Monterey 12 macos-12

macOS Big Sur 11 macos-latest or macos-11 The macos-latest label is currently transitioning to the macOS Monterey 12 runner image. During the transition, the label might refer to the runner image for either macOS 11 or 12. For more information, see this GitHub blog post.

macOS Catalina 10.15 [deprecated] macos-10.15 Migrate to macOS-11 or macOS-12. For more information, see this GitHub blog post.

## On - Events

<https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows>

### Repository Related

#### on: push

<https://learn.microsoft.com/en-us/training/paths/automate-workflow-github-actions/>

Workflow triggered hen a commit is pushed to a branch

`on: push`

#### on: pull_request, create, fork, issues, issue_comment, watch, discussion, etc

<https://learn.microsoft.com/en-us/training/paths/automate-workflow-github-actions/>

No notes

### Other

#### on: workflow_dispatch

Manually trigger a workflow
`on: workflow_dispatch`

#### on: repository_dispatch

Trigger a workflow by querying a REST API
`on: repository_dispatch`

#### on: schedule

Workflow triggered on a schedule

#### on: workflow_call

Called on by other workflows.

## Context

Context can be accessed using the `${{}}` expression.

E.g. `${{ github }}` or `${{ toJSON(github) }}`

<https://docs.github.com/en/actions/learn-github-actions/contexts>

Context name Type Description

github object Information about the workflow run. For more information, see github context.

env object Contains variables set in a workflow, job, or step. For more information, see env context.

vars object Contains variables set at the repository, organization, or environment levels. For more information, see vars context.

job object Information about the currently running job. For more information, see job context.

jobs object For reusable workflows only, contains outputs of jobs from the reusable workflow. For more information, see jobs context.

steps object Information about the steps that have been run in the current job. For more information, see steps context.

runner object Information about the runner that is running the current job. For more information, see runner context.

secrets object Contains the names and values of secrets that are available to a workflow run. For more information, see secrets context.

strategy object Information about the matrix execution strategy for the current job. For more information, see strategy context.

matrix object Contains the matrix properties defined in the workflow that apply to the current job. For more information, see matrix context.

needs object Contains the outputs of all jobs that are defined as a dependency of the current job. For more information, see needs context.

inputs object Contains the inputs of a reusable or manually triggered workflow. For more information, see inputs context.

## Functions

<https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/expressions#functions>

## Artifacts

### Upload

<https://github.com/marketplace/actions/upload-a-build-artifact>

### Download

<https://github.com/marketplace/actions/download-a-build-artifact>

## Contexts

<https://docs.github.com/en/actions/learn-github-actions/contexts#about-contexts>

### Get values from previous workflow steps

`needs` Contains the outputs of all jobs that are defined as a dependency of the current job. For more information, see needs context.

#### Set an output value

```YAML
build:
    needs: ...
    runs-on: ubuntu-latest
    outputs:
      script-file: ${{ steps.publish.outputs.script-file }}
    steps:
        ...
      - name: Publish JS filename
        id: publish
        run: find dist/assets/*.js -type f -execdir echo 'script-file={}' >> $GITHUB_OUTPUT ';'
        ...
```

#### Get an output value in another step/job

```YAML
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
        ...
      - name: Output filename
        run: echo "${{ needs.build.outputs.script-file }}"
        ...
```

## Caching

<https://github.com/marketplace/actions/cache>

The following should be used in all jobs that use the dependencies

```YAML
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/checkout@v3
      - name: Cache deps
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        run: npm ci
        ...
```

```YAML
...
      - name: Cache dependencies
        id: cache
        uses: actions/cache@v3
        with:
          path: node_modules
          key: deps-node-modules-${{ hashFiles('**/package-lock.json') }}
      - name: Install dependencies
        if: ${{ steps.cache.outputs.cache-hit != 'true' }}
        run: npm ci
      ...
```

## Environment Variables

Environment variables can be accessed in Node through `process.env`.

### Set env variables

#### For all jobs

```YAML
name: Deployment
on:
  push:
    branches:
      - main
      - dev
env:
  MONGODB_DB_NAME: gha-demo
...
```

#### Per job

```YAML
...
jobs:
  test:
    env:
      MONGODB_CLUSTER_ADDRESS: clusterA
      PORT: 8080
    environment: testing
    runs-on: ubuntu-latest
    ...
```

#### Using env variable in workflow

Using the environment variables will differ by the runner.

##### Linux and MacOS

Using `$ENV_NAME` or `${{ env.ENV_NAME }}`

##### Windows

Using `$env:ENV_NAME`

```YAML
jobs:
  test:
    env:
      MONGODB_CLUSTER_ADDRESS: clusterA
      PORT: 8080
    runs-on: ubuntu-latest
    steps:
      ...
      - name: Run server
        run: npm start & npx wait-on http://127.0.0.1:$PORT
      - name: Print results
        run: echo "Running on port: ${{ env.PORT }}"
    ...
```

### Default environment variables

<https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables>

### Repo-specific secrets

Repo-specific secrets can be used as values in your workflows.

#### Reference a secret in your workflow

```YAML
...
jobs:
  test:
    env:
      MONGODB_CLUSTER_ADDRESS: ${{ secrets.MONGODB_CLUSTER_ADDRESS }}
      PORT: 8080
    runs-on: ubuntu-latest
    ...
```

### Environment-specific secrets and variables

<https://docs.github.com/en/actions/learn-github-actions/contexts#vars-context>

Environments (Testing, staging, etc) can be created through `Settings > Environments`.

Environments can have their own stored secrets and variables so a new repo-specific secrets/variables is not needed for each environment.

Variables are not secret and can be viewed and altered.
Secrets are not visible and can only be updated.

Variables could be useful for things such as addresses and ports.
Secrets could be useful for things such as database passwords.

Declaring an environment is through the `environment` key.

#### Reference a environment in your job

```YAML
...
jobs:
  test:
    environment: testing
    env:
      MONGODB_CLUSTER_ADDRESS: ${{ secrets.MONGODB_CLUSTER_ADDRESS }}
    ...
```

When requesting a secret/variable in the job, it will retrieve the environment value instead of an overall repo-specific value.

## Controlling Execution Flow

### if

```YAML
...
      - name: Test code
        id: run-tests
        run: npm run test
      - name: Upload test report
        if: ${{ failure() && steps.run-tests.outcome == 'failure' }}
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
        ...
```

#### Note

`${{  }}` can be ignored in `if`

#### More on the `steps` context

<https://docs.github.com/en/actions/learn-github-actions/contexts#steps-context>

#### More on expressions

<https://docs.github.com/en/actions/learn-github-actions/expressions>

#### Status check functions

<https://docs.github.com/en/actions/learn-github-actions/expressions#status-check-functions>

### continue-on-error

Using `continue-on-error` to run a step when a previous step(s) has failed.

This could be similar to the previous `if: ${{ failure() }}`.
`if` will display that a job failed (red failure icon) if you are using the above `if` method to continue after a failure.

`continue-on-error` will display a successful job icon if a previous step failed.

```YAML
...
      - name: Test code
        id: run-tests
        run: npm run test
      - name: Upload test report
        continue-on-error: true
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: test.json
        ...
```

## Matrix

Matrix allows you to run the job multiple times for each value in the matrix. A matrix could have a length of 1, in which case, the job will only run once.

```YAML
...
on: push
jobs:
  build:
    strategy:
      matrix:
        node-version: [12, 14, 16]
        os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - name: Get code
      - uses: actions/checkout@v3
      - name: Install node
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      ...
```

### Include/Exclude

```YAML
...
jobs:
  build:
    strategy:
      matrix:
        node-version: [12, 14, 16]
        os: [ubuntu-latest, windows-latest]
        include:
          - node-version: 18
            operating-system: ubuntu-latest
        exclude:
          - node-version: 12
            operating-system: windows-latest
    runs-on: ${{ matrix.os }}
    ...
```

## Reusable workflows

You can reference another workflow to create reusable workflows.

```YAML
...
  deploy:
    needs: build
    uses: ./.github/workflows/reusable-demo.yml
    ...
```

The reusable workflow should be run with `workflow_call` in the `on` section.

## Inputs

<https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions>

### Using workflow

```YAML
...
on:
  workflow_call:
    inputs:
      artifact-name:
        description: The name of the deployable artifact
        required: false
        default: dist
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Get code
        uses: actions/download-artifact@v3
        with:
          name: $ {{ inputs.artifact-name }}
      ...
```

### Providing workflow

```YAML
...
  deploy:
    needs: build
    uses: ./.github/workflows/reusable-demo.yml
    with:
      artifact-name: dist-files
    ...
```

### Providing secrets

```YAML
...
  deploy:
    needs: build
    uses: ./.github/workflows/reusable-demo.yml
    secrets:
      some-secret: ${{ secrets.some-secret }}
    ...
```

## Outputs

### Providing outputs

```YAML
name: Reusable
on:
  workflow_call:
    outputs:
      result:
        description: The result of the deployment operation
        value: ${{ jobs.deploy.outputs.outcome }}
jobs:
  deploy:
    outputs:
      outcome: ${{ steps.step-result.outputs.step-result }}
    runs-on: ubuntu-latest
    steps:
      ...
      - name: Set result output
        id: step-result
        run: echo "step-result=success" >> $GITHUB_OUTPUT
    ...
```

### Using outputs

The following should run AFTER the previous workflow has finished.

```YAML
...
jobs:
  ...
  print-deploy-result:
    needs: deploy
    runs-on: ubuntu-latest
    steps:
      - name: Print deploy out
        run: echo "${{ needs.deploy.outputs.result }}"
```
