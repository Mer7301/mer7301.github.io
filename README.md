# belfry-project.github.io

Source of [belfry-project.github.io](https://belfry-project.github.io/): the comparison of Belfry with the last AWX release, and the [documentation](https://belfry-project.github.io/docs/).

- `changes.yml`: the curated list of changes, with links to the pull requests. Add an entry here for every change users should know about.
- `build.py`: reads component versions from the source of AWX 24.6.1 / awx-operator 2.19.1 and of Belfry `main`, looks up support status on [endoflife.date](https://endoflife.date), and writes `_site/index.html` from `template.html`.
- `scan.py`: scans the AWX 24.6.1 and Belfry images with [Trivy](https://trivy.dev) and writes `scan-summary.json`, which `build.py` adds to the page.
- `mkdocs.yml`, `docs/`: the Belfry guides (install, migrating from AWX, operations), published under `/docs/`. Built with MkDocs and Material, pinned in `requirements-docs.txt`. Release-specific values (version, manifest URL, Helm repository) are only in the `extra:` section of `mkdocs.yml`; update them with each release.
- `.github/workflows/pages.yml`: builds and publishes the site on every push to `main`, daily, and on demand. It also builds the product documentation from the `main` branches of [belfry](https://github.com/belfry-project/belfry) (`docs/docsite`, Sphinx, under `/docs/belfry/`) and [belfry-operator](https://github.com/belfry-project/belfry-operator) (`docs/`, MkDocs, under `/docs/operator/`), so changes there are published by the next daily build, or by running the workflow by hand.

Build locally:

```bash
pip install pyyaml
GITHUB_TOKEN=$(gh auth token) python build.py   # the token avoids API rate limits
```

Build the guides locally:

```bash
pip install -r requirements-docs.txt
mkdocs serve            # http://127.0.0.1:8000/docs/
mkdocs build --strict   # writes _site/docs, as in CI
```
