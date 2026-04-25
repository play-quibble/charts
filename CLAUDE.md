# play-quibble/charts

Helm chart repository for the Quibble trivia platform. Charts are published to GitHub Pages via `helm/chart-releaser-action` and served at `https://play-quibble.github.io/charts`.

## Structure

- `charts/quibble/` — Helm chart for the full Quibble stack (API, web frontend, Redis)
- `.github/workflows/` — CI/CD pipelines

## Common tasks

### Lint a chart
```
helm lint charts/quibble
```

### Render templates
```
helm template quibble charts/quibble -f charts/quibble/values.yaml
```

### Bump versions
Edit `charts/quibble/Chart.yaml`:
- `version` — bump for any chart template or values change (SemVer)
- `appVersion` — update when the upstream application releases a new version (matches the tag in `play-quibble/trivia`)

### Release process
Merging to `main` with changes under `charts/**` triggers `chart-release.yml`, which packages the chart, creates a GitHub Release with the `.tgz` artifact, and updates the Helm repo index on `gh-pages`.

### Adding a new chart
Create a directory under `charts/` following the same structure as `quibble/`.

## CI workflows

| Workflow | Trigger | Purpose |
|---|---|---|
| `chart-release.yml` | push main (`charts/**`) | Package and publish charts |
| `lint-test.yml` | PR (`charts/**`) | Lint and install-test changed charts via `ct` |
| `security.yml` | push main + PR | Checkov IaC policy, Trivy misconfiguration, Gitleaks, dependency review |
| `scorecard.yml` | weekly + push main | OpenSSF supply chain security scorecard |
| `megalinter.yml` | push + PR | YAML, Markdown, Actions, JSON, Kubernetes linting |
