# Contributing

Belfry is a community project, and every contribution helps: a bug report, a migration report, a docs fix or a pull request.

## Repositories

| Repository | What | Based on |
|---|---|---|
| [belfry](https://github.com/belfry-project/belfry) | The application: REST API, task engine, `awx.awx` collection, `awxkit` CLI, and the [manual](https://belfry-project.github.io/docs/belfry/) | ansible/awx |
| [belfry-operator](https://github.com/belfry-project/belfry-operator) | The Kubernetes operator, the upgrade test, and the [operator reference](https://belfry-project.github.io/docs/operator/) | ansible/awx-operator |
| [belfry-ee](https://github.com/belfry-project/belfry-ee) | The default execution environment | ansible/awx-ee |
| [belfry-ui](https://github.com/belfry-project/belfry-ui) | The web UI | ansible/ansible-ui |
| [helm-charts](https://github.com/belfry-project/helm-charts) | The Helm chart, generated from the operator | ansible-community/awx-operator-helm |
| [belfry-project.github.io](https://github.com/belfry-project/belfry-project.github.io) | This site: these guides and the comparison page | |
| [.github](https://github.com/belfry-project/.github) | Organization profile, security policy, decisions, shared Renovate preset | |

## Issues

- Bugs and feature requests: open an issue in the repository of the component. If you are not sure which, use [belfry](https://github.com/belfry-project/belfry/issues).
- Include the Belfry version (`/api/v2/ping/`), the operator version, your Kubernetes distribution, and the relevant logs.
- Migration reports are very welcome, also when everything worked: open an issue in [belfry-operator](https://github.com/belfry-project/belfry-operator/issues) with your starting version and setup.
- Vulnerabilities: never in a public issue, see [Security](security.md).

## Development environment

The application is developed with the docker-compose environment from AWX. You need Docker (or Podman with docker-compose), `make`, Git and about 8 GB of memory:

```bash
git clone https://github.com/belfry-project/belfry.git
cd belfry
make docker-compose-build   # optional: otherwise ghcr.io/belfry-project/belfry_devel:main is used
make docker-compose
```

Then, in another terminal, create an admin user and open <https://localhost:8043>:

```bash
docker exec -ti tools_awx_1 awx-manage createsuperuser
```

The full instructions, including the UI, a Keycloak or LDAP server, execution nodes and debugging, are in [`tools/docker-compose/README.md`](https://github.com/belfry-project/belfry/blob/main/tools/docker-compose/README.md).

For the operator, see its [development guide](https://belfry-project.github.io/docs/operator/development.html). It needs a Kubernetes cluster, for example kind.

## Pull requests

- Make a branch, and open a pull request to `main`. Fill in the pull request template.
- Keep changes small and focused: Belfry merges upstream AWX changes when there are any, and small diffs keep that easy.
- belfry follows `ruff` (`ruff check` and `ruff format --check`, configured in `pyproject.toml`).
- Add an entry to [`changes.yml`](https://github.com/belfry-project/belfry-project.github.io/blob/main/changes.yml) for changes users should know about; it feeds the [comparison page](https://belfry-project.github.io/).

## CI

Everything is built and tested on GitHub Actions:

| Repository | What runs |
|---|---|
| belfry | Linting, API and unit tests, functional tests with PostgreSQL, the collection and awxkit tests, an API schema check, the docs build, and a test deployment on Kubernetes. Images are built for `main` and published as `:main`. |
| belfry-operator | `molecule` tests on kind, the [upgrade test](migrate.md#what-the-upgrade-test-covers) from AWX {{ awx.version }} every week, and the operator image build. |
| belfry-ee | The EE build for amd64 and arm64; pull requests publish `belfry-ee:pr-<n>`, which belfry's CI can test against. |
| helm-charts | Chart linting and an install on kind. |
| belfry-project.github.io | This site, rebuilt daily, with a vulnerability scan of the AWX and Belfry images. |

Dependency updates come from [Renovate](https://docs.renovatebot.com/), configured by the shared preset in [`.github/default.json`](https://github.com/belfry-project/.github/blob/main/default.json).
