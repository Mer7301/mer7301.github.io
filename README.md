# belfry-project.github.io

Source of [belfry-project.github.io](https://belfry-project.github.io/): the comparison of Belfry with the last AWX release.

- `changes.yml`: the curated list of changes, with links to the pull requests. Add an entry here for every change users should know about.
- `build.py`: reads component versions from the source of AWX 24.6.1 / awx-operator 2.19.1 and of Belfry `main`, looks up support status on [endoflife.date](https://endoflife.date), and writes `_site/index.html` from `template.html`.
- `scan.py`: scans the AWX 24.6.1 and Belfry images with [Trivy](https://trivy.dev) and writes `scan-summary.json`, which `build.py` adds to the page.
- `.github/workflows/pages.yml`: builds and publishes the site on every push to `main`, daily, and on demand.

Build locally:

```bash
pip install pyyaml
GITHUB_TOKEN=$(gh auth token) python build.py   # the token avoids API rate limits
```
