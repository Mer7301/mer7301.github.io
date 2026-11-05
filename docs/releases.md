# Releases and support

## Versions

Belfry uses [semantic versioning](https://semver.org/), starting at **1.0.0** ([decision](https://github.com/belfry-project/.github/blob/main/DECISIONS.md)). Belfry does not continue AWX's numbering (24.x): it is a separate product, and its compatibility promises start at 1.0.

- **Major** (2.0.0): may contain breaking changes, such as removed API fields, settings or `AWX` resource fields. The release notes say what to do.
- **Minor** (1.1.0): new features, dependency updates, database migrations. Upgrades from the previous minor release work without changes.
- **Patch** (1.0.1): fixes and security updates only.

The application (`belfry`), the execution environment (`belfry-ee`) and the operator (`belfry-operator`) are released together, with the same version. The Helm chart has its own version; its `appVersion` is the operator version it installs.

Release tags have no `v` prefix (`1.0.0`). Release candidates are tagged `X.Y.Z-rc.N` and are for testing.

## Release notes

Each repository has its release notes on its GitHub releases page, grouped by the labels of the pull requests:

- [belfry](https://github.com/belfry-project/belfry/releases): the application, plus the `awx.awx` collection and the `awxkit` CLI as release assets
- [belfry-operator](https://github.com/belfry-project/belfry-operator/releases): the operator, with the install manifest
- [belfry-ee](https://github.com/belfry-project/belfry-ee/releases): the execution environment
- [helm-charts](https://github.com/belfry-project/helm-charts/releases): the Helm chart

The [comparison page](https://belfry-project.github.io/) summarizes all changes compared with AWX {{ awx.version }}.

## Image tags { #image-tags }

| Tag | Meaning | Use it for |
|---|---|---|
| `:X.Y.Z`, e.g. `:{{ release.version }}` | A release. Never moves. | Production. Better still, pin the digest. |
| `:latest` | The newest final release (not release candidates). Moves with every release. | Trying out the current release. |
| `:main` | The newest build of the `main` branch. Moves with every merge. | Testing and development only. |
| `:main-<sha>` (belfry-ee) | A build of a specific `main` commit. | Reproducing a `main` build. |
| `:pr-<n>` (belfry-ee) | A test build of a pull request. | Testing that pull request only. |

All images are in [`ghcr.io/belfry-project`](https://github.com/orgs/belfry-project/packages), for amd64 and arm64.

## Support

Belfry is maintained by volunteers. There are no service-level guarantees, but:

- the **latest release** gets fixes and security updates, as patch releases;
- older releases are not updated: upgrade to the latest release, which is supported from any earlier 1.x release;
- upgrades from **AWX {{ awx.version }}** are [tested](migrate.md#what-the-upgrade-test-covers) every week.

Report bugs as [issues](https://github.com/belfry-project/belfry/issues), and vulnerabilities [privately](security.md).
