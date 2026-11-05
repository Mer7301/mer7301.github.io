# FAQ

## Is Belfry a fork of AWX?

It continues AWX: the repositories are copies of the AWX repositories, with their full history, from AWX's development branch as of November 2026. That branch contains more than two years of development that AWX never released. Belfry is maintained independently, and is not affiliated with Red Hat or the Ansible project.

## Why is it not called AWX?

AWX, Ansible and the AWX logo are trademarks of Red Hat. A project that is not Red Hat's cannot release software under that name. *Belfry* is a bell tower, a nod to Ansible Tower, where AWX comes from.

## Why do I still see "AWX" everywhere?

For compatibility, Belfry keeps AWX's internal names: the `AWX` resource kind and the `awx.ansible.com` API group, the `awx-...` object names, the `awx.awx` collection, the `awx` CLI command, the Python package, the API paths and the managed EE name `AWX EE (latest)`. Renaming them would break existing installations, playbooks and scripts. Only user-facing names and the logo have changed.

## Will my playbooks, `awx.awx` modules and API scripts keep working?

The API is the same, so scripts and the `awx.awx` collection keep working. Playbooks run in a newer ansible-core ({{ release.ansible_core }} instead of {{ awx.ansible_core }}), which can need changes: see the [porting guides](migrate.md#7-update-execution-environments-and-playbooks). You can keep an older EE for playbooks that are not ported yet.

## Can I go back to AWX after migrating?

Only by restoring the backup you took before migrating, with awx-operator. See [Rolling back](migrate.md#rolling-back). Changes made in Belfry after the migration are then lost.

## Does Belfry work without Kubernetes?

Like AWX, Belfry is only supported on Kubernetes, with the operator. The `docker-compose` environment is for development only, see [Contributing](contributing.md).

## Can I run Belfry and AWX in the same cluster?

In different namespaces, yes. Both operators manage the same resource kinds, so do not run both in the same namespace. The CRDs are cluster-wide and shared: belfry-operator's CRDs add fields and a few validations to those of awx-operator {{ awx.operator_version }}, so AWX keeps working with them (one validation: `image` and `image_version` must be set together, or neither).

## Which UI does Belfry use?

The Ansible UI that AWX's development branch uses ([belfry-ui](https://github.com/belfry-project/belfry-ui)), built from ansible-ui as of September 2024.

## Where are the collection and CLI?

On the [belfry GitHub releases](https://github.com/belfry-project/belfry/releases), not on Galaxy or PyPI:

```bash
ansible-galaxy collection install https://github.com/belfry-project/belfry/releases/download/{{ release.version }}/awx-awx-{{ release.version }}.tar.gz
pip install https://github.com/belfry-project/belfry/releases/download/{{ release.version }}/awxkit-{{ release.version }}-py3-none-any.whl
```

## Is it free?

Yes. Belfry is under the same licenses as AWX (Apache 2.0 for most of it). All images, charts and docs are public.

## How can I help?

Try the migration and report how it went, report bugs, or send a pull request. See [Contributing](contributing.md).
