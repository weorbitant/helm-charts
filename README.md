# Helm Charts Repository

This repository contains Helm charts that are automatically published using GitHub Actions and served through GitHub Pages.

## Usage

### Add the repository

```bash
helm repo add orbitant https://weorbitant.github.io/helm-charts
helm repo update
```

### Search available charts

```bash
helm search repo orbitant
```

### Install a chart

```bash
helm install my-release orbitant/<chart-name>
```

## Development

### Repository structure

```
.
├── charts/                 # Directory for your Helm charts
│   └── <chart-name>/      # Each chart in its own directory
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
└── .github/
    └── workflows/
        └── release.yml    # GitHub Actions workflow for releases
```

### Add a new chart

1. Create a new directory inside `charts/`:

   ```bash
   mkdir -p charts/my-chart
   cd charts/my-chart
   helm create .
   ```

2. Edit `Chart.yaml` and update the version
3. Commit and push to the `main` branch
4. GitHub Actions will automatically:
   - Package the chart
   - Create a GitHub release
   - Update the Helm index on GitHub Pages

### Update an existing chart

1. Modify the necessary files in `charts/<chart-name>/`
2. Increment the version in `Chart.yaml` (following [SemVer](https://semver.org/))
3. Commit and push to the `main` branch
4. The workflow will automatically publish the new version

## How it works

This repository uses:

- **[chart-releaser-action](https://github.com/helm/chart-releaser-action)**: Automates chart packaging and publishing
- **GitHub Pages**: Serves the Helm chart repository
- **GitHub Releases**: Stores the chart packages

When you push to `main`:

1. The workflow detects changes in charts (based on new versions in `Chart.yaml`)
2. Packages the modified charts
3. Creates GitHub releases with the `.tgz` files
4. Updates `index.yaml` in the `gh-pages` branch
5. GitHub Pages serves the repository at `https://orbitant.github.io/helm-charts`

## GitHub Pages Configuration

**Important**: You must configure GitHub Pages to use the `gh-pages` branch:

1. Go to Settings → Pages in your repository
2. Under "Source", select the `gh-pages` branch and `/ (root)` folder
3. Save the changes

The first time you run the workflow, the `gh-pages` branch will be created automatically.

## Requirements

- Charts must follow [Helm best practices](https://helm.sh/docs/chart_best_practices/)
- Each chart must have a valid `Chart.yaml` with semantic versioning
- Versions must be incremented for new releases to be published
