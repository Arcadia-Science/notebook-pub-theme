# GitHub Actions

This repo contains two reusable workflows that automate theme updates across all notebook pubs.

## Workflows

### check-theme-updates.yml

Checks for new theme releases and opens a PR if an update is available.

**Trigger:** Daily at 9AM PST, or manual dispatch

**What it does:**
1. Compares version in `_extensions/Arcadia-Science/arcadia-pub-theme/_extension.yml` against the latest GitHub release
2. If a newer version exists, runs `make update-theme`
3. Opens a PR with the updated extension files

**Used by:** Each notebook pub has a thin wrapper that calls this workflow:

```yaml
name: Check Theme Updates

on:
  schedule:
    - cron: '0 17 * * *'
  workflow_dispatch:

jobs:
  check:
    uses: Arcadia-Science/notebook-pub-theme/.github/workflows/check-theme-updates.yml@main
    secrets: inherit
```

### publish-theme-change.yml

Syncs theme changes to the `publish` branch and re-renders the site.

**Trigger:** When `_extensions/**` changes on `main`

**What it does:**
1. Copies `_extensions/` from `main` to `publish` branch
2. Commits and pushes to `publish`
3. Runs `quarto publish gh-pages` to deploy the site

**Used by:** Each notebook pub has a thin wrapper that calls this workflow:

```yaml
name: Publish Theme Change

on:
  push:
    branches: [main]
    paths: ['_extensions/**']
  workflow_dispatch:

jobs:
  publish:
    uses: Arcadia-Science/notebook-pub-theme/.github/workflows/publish-theme-change.yml@main
    secrets: inherit
```

## Why reusable workflows?

The workflow logic lives here so that changes propagate automatically. When we update how PRs are formatted, which reviewer is tagged, or how publishing works, all pubs get the update without needing individual PRs.

Each pub's wrapper files rarely need to change—they just define the trigger (schedule, path filter) and delegate to the reusable workflow.

## Why a pull model?

We use a "pull" model where each pub checks for updates independently, rather than a "push" model where the theme repo would dispatch updates to all pubs.

**Pull model** (what we use):
- Each pub runs a daily check for new theme releases
- Only requires adding wrapper workflows to the template
- New pubs inherit the workflows automatically

**Push model** (alternative we avoided):
- Theme repo would trigger workflows in all pub repos
- Requires maintaining a list of all pub repos in the theme repo
- Requires a PAT with write access to all pub repos
- Must update the list whenever a new pub is created

The pull model scales better and requires no central coordination.

## Related documentation

For content release workflows (`build.yml`, `publish.yml`), see the [notebook-pub-template developer docs](https://github.com/Arcadia-Science/notebook-pub-template/blob/main/developer-docs/GITHUB_ACTIONS.md).
