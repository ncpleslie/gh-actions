# gh-actions

Basic notes on GitHub Actions

## Runs on - Hosted runners

https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners

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

https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows

### Repository Related

#### on: push

https://learn.microsoft.com/en-us/training/paths/automate-workflow-github-actions/

Workflow triggered hen a commit is pushed to a branch

`on: push`

#### on: pull_request, create, fork, issues, issue_comment, watch, discussion, etc

https://learn.microsoft.com/en-us/training/paths/automate-workflow-github-actions/

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

https://docs.github.com/en/actions/learn-github-actions/contexts

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

https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/expressions#functions

## Artifacts

### Upload

https://github.com/marketplace/actions/upload-a-build-artifact

### Download

https://github.com/marketplace/actions/download-a-build-artifact

## Contexts

https://docs.github.com/en/actions/learn-github-actions/contexts#about-contexts

### Get values from previous workflow steps

`needs` Contains the outputs of all jobs that are defined as a dependency of the current job. For more information, see needs context.
