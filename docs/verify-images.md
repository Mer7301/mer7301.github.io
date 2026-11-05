# Verifying images

Every Belfry image is built by GitHub Actions in a public repository, and published with two signed [attestations](https://docs.github.com/en/actions/security-for-github-actions/using-artifact-attestations):

- **Build provenance**: which repository, commit and workflow run built the image.
- **SBOM** (SPDX): every package in the image, with its version. The development images `belfry_devel` and `belfry_kube_devel` only have provenance.

The attestations are signed with [Sigstore](https://www.sigstore.dev/) and stored in the registry next to the image, and on GitHub.

## Verify the provenance

With the [GitHub CLI](https://cli.github.com/):

```bash
gh attestation verify oci://ghcr.io/belfry-project/belfry:{{ release.version }} --owner belfry-project
gh attestation verify oci://ghcr.io/belfry-project/belfry-ee:{{ release.version }} --owner belfry-project
gh attestation verify oci://ghcr.io/belfry-project/belfry-operator:{{ release.version }} --owner belfry-project
```

The command exits with an error if the image has no valid attestation from a `belfry-project` repository. Add `--format json` to see the repository, commit and workflow. To require that the image comes from a specific repository:

```bash
gh attestation verify oci://ghcr.io/belfry-project/belfry:{{ release.version }} --repo belfry-project/belfry
```

Verify the image you actually deploy by its digest, for example `oci://ghcr.io/belfry-project/belfry@sha256:...`. A tag can move; a digest cannot.

In a cluster, policy engines such as [Kyverno](https://kyverno.io/) or the [Sigstore policy controller](https://docs.sigstore.dev/policy-controller/overview/) can check the attestations before a pod starts.

## SBOMs

Download the SBOM of an image:

```bash
gh attestation verify oci://ghcr.io/belfry-project/belfry-ee:{{ release.version }} --owner belfry-project \
  --predicate-type https://spdx.dev/Document/v2.3 --format json \
  --jq '.[0].verificationResult.statement.predicate' > belfry-ee.spdx.json
```

Feed it to your scanner, for example `trivy sbom belfry-ee.spdx.json` or `grype sbom:belfry-ee.spdx.json`. Release SBOMs are also attached to the [GitHub releases](https://github.com/belfry-project/belfry/releases).

## Known vulnerabilities

The images of AWX {{ awx.version }} and of Belfry are scanned with [Trivy](https://trivy.dev/) every day, and the result is shown on the [comparison page](https://belfry-project.github.io/#vulnerabilities). Belfry dependencies are updated through [Renovate](https://docs.renovatebot.com/) pull requests, which go through CI. To report a vulnerability, see [Security](security.md).
