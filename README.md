# gh-actions

Basic notes on GitHub Actions

## Runs on

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
