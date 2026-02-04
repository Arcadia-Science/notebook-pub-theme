# Publishing Automations

This document explains how notebook pubs are automatically published, including the interaction between content releases and theme updates.

## Two Publishing Paths

Notebook pubs have two independent paths to publication:

### Content Release Path

Triggered by pushing a version tag (e.g., `v1.0`):

```
Tag push → build.yml → _build.py → PR to publish → merge → publish.yml → gh-pages
```

This path:
- Runs `_build.py` to assemble versioned notebooks
- Creates a PR targeting the `publish` branch
- On merge, renders the site and deploys to gh-pages
- Creates a new content version visible in the version dropdown

### Theme Update Path

Triggered daily (or manually) when a new theme release is detected:

```
Daily check → check-theme-updates.yml → PR to main → merge → publish-theme-change.yml → gh-pages
```

This path:
- Checks for new theme releases from `notebook-pub-theme`
- Creates a PR to update `_extensions/` on `main`
- On merge, syncs the theme to `publish` and re-renders the site
- Does NOT create a new content version—theme updates are orthogonal to scientific content

## Workflow Details

### check-theme-updates.yml

Runs daily at midnight PST. Compares the version in `_extensions/Arcadia-Science/arcadia-pub-theme/_extension.yml` against the latest GitHub release. If an update is available, runs `make update-theme` and opens a PR.

The workflow logic lives in `notebook-pub-theme` as a reusable workflow. Each pub has a thin wrapper that calls it.

### publish-theme-change.yml

Triggered when changes to `_extensions/**` are pushed to `main`. Syncs the theme to the `publish` branch and re-renders the site.

## The Guard: Why Theme Updates Skip During Content Releases

`publish-theme-change.yml` checks for open PRs targeting the `publish` branch. If any exist, it exits early without syncing or publishing.

### Why?

When a content release is in progress, there's an open PR from a `build-*` branch to `publish`. If we sync the theme and publish at this moment:

1. **Redundant deploys**: The theme sync would deploy to gh-pages, then the content release would deploy again shortly after.

2. **Potential conflicts**: If both workflows push to gh-pages concurrently, the pushes could conflict.

3. **The theme isn't lost**: When the content PR eventually merges, git preserves the theme. Here's why:

```
Common ancestor:  content A + theme v1.0
build-* branch:   content A + theme v1.0 + versioned notebooks (no _extensions changes)
publish branch:   content A + theme v1.1 (_extensions updated by theme sync)
```

Since `build-*` didn't modify `_extensions/`, git's 3-way merge takes `publish`'s version. The versioned notebooks merge cleanly. Result: both the new content AND the theme update survive.

4. **Simpler mental model**: Rather than reasoning about merge behavior and concurrent deploys, we simply wait. The theme update is already on `main` and will be included in the next publish—whether that's the content release completing, or a future theme sync after the content release merges.

### When Does the Theme Actually Get Published?

If the guard triggers (content release in progress):
- Theme is on `main` but not yet on `publish` or gh-pages
- When content PR merges → `publish.yml` renders and deploys (includes theme if it merged cleanly, or next theme sync picks it up)

If no content release in progress:
- Theme syncs to `publish` immediately
- Site re-renders and deploys with new theme

## Branch State Reference

| Branch | Purpose |
|--------|---------|
| `main` | Development branch. Theme updates merge here first. |
| `publish` | Publication-ready state. Content releases and theme syncs target this. |
| `build-*` | Temporary branches created by `build.yml` with versioned notebooks. |
| `gh-pages` | Rendered site. Quarto publishes here. |

## Debugging

**Theme update PR not appearing?**
- Check Actions → "Check Theme Updates" for errors
- Verify the theme version in `_extension.yml` vs latest release
- Manually trigger the workflow from Actions tab

**Theme not deploying after PR merge?**
- Check if a content release PR is open (guard will skip)
- Check Actions → "Publish Theme Change" for the skip message
- The theme will deploy when the content release completes

**Site shows old theme after content release?**
- The theme sync may have been skipped during the release
- Manually trigger "Check Theme Updates" or wait for daily run
