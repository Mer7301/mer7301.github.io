# Upgrading Belfry

A Belfry upgrade works like an awx-operator upgrade: you install the new belfry-operator release, and it upgrades every `AWX` resource in its namespace to the Belfry version it was released with.

## Before you upgrade

1. Read the [release notes](https://github.com/belfry-project/belfry/releases) of every release between yours and the new one. Breaking changes are only made in a new major version, see [Releases and support](releases.md).
2. [Take a backup](backup-restore.md#taking-a-backup).
3. Check that your `AWX` resource does not pin `image` and `image_version`, unless you want to upgrade them yourself.

Each release of belfry, belfry-ee and belfry-operator has the same version number, and the operator of a release deploys the application and EE images of that release.

## Upgrade the operator

=== "Release manifest"

    ```bash
    kubectl apply --server-side -f https://github.com/belfry-project/belfry-operator/releases/download/<version>/belfry-operator.yaml
    ```

=== "Kustomize"

    Change the `ref` and `newTag` in your `kustomization.yaml` to the new version, then:

    ```bash
    kubectl apply -k .
    ```

=== "Helm"

    Apply the CRDs first ([why](#helm)), then upgrade the chart:

    ```bash
    kubectl apply --server-side --force-conflicts \
      -k "https://github.com/belfry-project/belfry-operator/config/crd?ref=<version>"
    helm repo update
    helm upgrade belfry-operator {{ release.helm_repo_name }}/{{ release.helm_chart }} -n awx --version <chart version> -f values.yaml
    ```

### Helm and CRDs { #helm }

Helm installs the CRDs from the chart's `crds/` directory on `helm install`, but never updates them on `helm upgrade`. Apply them from the operator release before every chart upgrade, as above. `--force-conflicts` takes over the fields that Helm set.

## Watch the upgrade

Then watch the upgrade:

```bash
kubectl -n awx rollout status deployment/awx-operator-controller-manager
kubectl -n awx get pods,jobs -w
kubectl -n awx get awx -o custom-columns=NAME:.metadata.name,VERSION:.status.version,FROM:.status.upgradedFrom
```

The operator runs the database migrations in a Job (`<name>-migration-<hash>`) before it rolls out the new web and task pods.

## Upgrading instances one at a time

With `auto_upgrade: false` in an `AWX` resource, the operator leaves a running instance alone. To upgrade it later, set `auto_upgrade: true`, wait for the upgrade, and set it back. See [auto upgrade](https://belfry-project.github.io/docs/operator/user-guide/advanced-configuration/auto-upgrade.html) in the operator reference.

## Following `main`

The `:main` images are built from every commit on `main`. They are for testing and development, not for production: they can contain unreleased database migrations, and there is no supported way back to a release from them. To try them, deploy an operator built from `main` (`?ref=main` and `newTag: main` in Kustomize), which deploys `ghcr.io/belfry-project/belfry:main`.
