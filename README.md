# gh-actions

Basic notes on GitHub Actions

## Runs on - Hosted runners

https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners

Runner image	YAML workflow label	Notes

Windows Server 2022	windows-latest or windows-2022	The windows-latest label currently uses the Windows Server 2022 runner image.

Windows Server 2019	windows-2019	

Ubuntu 22.04	ubuntu-latest or ubuntu-22.04	The ubuntu-latest label currently uses the Ubuntu 22.04 runner image.

Ubuntu 20.04	ubuntu-20.04	

Ubuntu 18.04 [deprecated]	ubuntu-18.04	Migrate to ubuntu-20.04 or ubuntu-22.04. For more information, see this GitHub blog post.

macOS Monterey 12	macos-12

macOS Big Sur 11	macos-latest or macos-11	The macos-latest label is currently transitioning to the macOS Monterey 12 runner image. During the transition, the label might refer to the runner image for either macOS 11 or 12. For more information, see this GitHub blog post.

macOS Catalina 10.15 [deprecated]	macos-10.15	Migrate to macOS-11 or macOS-12. For more information, see this GitHub blog post.

## On - Events

https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows

### Repository Related

#### on: push

Workflow triggered hen a commit is pushed to a branch

`on: push`

#### on: pull_request, create, fork, issues, issue_comment, watch, discussion, etc

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
