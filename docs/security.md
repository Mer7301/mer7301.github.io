# Security

## Reporting a vulnerability

Report vulnerabilities **privately**, through GitHub's private vulnerability reporting: open the **Security** tab of the affected repository and choose **Report a vulnerability**.

| Component | Report in |
|---|---|
| The application, API, collection and CLI | [belfry](https://github.com/belfry-project/belfry/security/advisories/new) |
| The Kubernetes operator | [belfry-operator](https://github.com/belfry-project/belfry-operator/security/advisories/new) |
| The execution environment image | [belfry-ee](https://github.com/belfry-project/belfry-ee/security/advisories/new) |
| The Helm chart | [helm-charts](https://github.com/belfry-project/helm-charts/security/advisories/new) |
| The web UI | [belfry-ui](https://github.com/belfry-project/belfry-ui/security/advisories/new) |

Do not report vulnerabilities in public issues, pull requests or discussions. Include the affected component and versions or image digests, steps to reproduce, the impact, and a fix if you have one.

We aim to acknowledge a report within five working days, keep you informed, and agree on a disclosure date with you. Fixes are announced in a GitHub security advisory, with a CVE where appropriate, and reporters who want to be are credited. Belfry is maintained by volunteers, so these are goals rather than guarantees.

If a vulnerability also affects AWX or Ansible, we tell you, and with your agreement coordinate disclosure with the Ansible security team.

The full policy is [SECURITY.md](https://github.com/belfry-project/.github/blob/main/SECURITY.md).

## Supported versions

The latest Belfry release receives security fixes. See [Releases and support](releases.md).

## Staying informed

- **Watch** the repositories you use on GitHub, with *Custom > Security alerts* and *Releases*, to be notified of advisories and fixes.
- The [comparison page](https://belfry-project.github.io/#vulnerabilities) shows the known vulnerabilities in the current images, rescanned daily.
- [Verify the images](verify-images.md) you deploy, and scan their SBOMs with your own tools.

## Hardening notes

These are the same as for AWX:

- Change the generated admin password, or set your own with `admin_password_secret`, and use an identity provider for users.
- Expose Belfry only over TLS (see [Installing](install.md#4-expose-the-service)), and set `csrf_cookie_secure` and `session_cookie_secure` in the `AWX` resource.
- Keep the `<name>-secret-key` secret and your backups safe: with them, anyone can decrypt every credential.
- Run jobs in container groups or on execution nodes with only the network access they need.
