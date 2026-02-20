# notebook-pub-theme

A [Quarto extension](https://quarto.org/docs/extensions/) that provides shared styling and infrastructure for Arcadia Science notebook publications.

This extension provides all the styling for our notebook pubs. It includes our assets (citation style, logo files, etc), all of the CSS (fonts, page layout, etc.), syntax highlighting for code blocks, and interactive components (mini-title sticky header, author reveal, citation box, logo animation, etc.).

## How to use

This extension comes pre-packaged with our [notebook pub template](https://github.com/Arcadia-Science/notebook-pub-template). **Head there if you're trying to develop a notebook pub**, or continue reading if you plan to make changes to the theme shared across all our notebook pubs.

## Deploying theme changes

Theme updates propagate automatically to all notebook pubs (including the template). Here's how to make and deploy changes:

### 1. Make and test your changes

Clone this repo and [notebook-pub-test](https://github.com/Arcadia-Science/notebook-pub-test). Make sure you've [installed Quarto](https://quarto.org/docs/get-started/).

Replace the test repo's extension with a symlink to your local theme:

```bash
cd notebook-pub-test
rm -rf _extensions/Arcadia-Science
ln -s /path/to/notebook-pub-theme/_extensions/Arcadia-Science _extensions/Arcadia-Science
```

Run `make preview` and make changes to CSS, JS, or other extension files in `notebook-pub-theme`. The preview will hot reload.

The syntax highlighting theme for code blocks lives in `arcadia-light.theme`. To create or edit a theme visually, use the [syntax highlighting theme builder](https://arcadia-science.github.io/arcadia-syntax-highlighting/) and export the Pandoc `.theme` file.

### 2. Create a release

When you're ready to deploy, you need to:

1. **Decide on a version number.** This repo uses [semantic versioning](https://semver.org/).

2. **Update the version in `_extensions/Arcadia-Science/arcadia-pub-theme/_extension.yml`:**

   ```yaml
   version: X.Y.Z
   ```

3. **Add an entry to `CHANGELOG.md`** describing what changed.

4. **Open a pull request** with your changes and merge it.

5. **Create a GitHub release** from `main`:

   ```bash
   gh release create vX.Y.Z --title "vX.Y.Z" --notes "Description of changes"
   ```

### 3. PRs are created automatically

Each notebook pub checks for theme updates daily. When a new release is detected, a PR is automatically opened. You can also trigger the check manually from the Actions tab → "Check Theme Updates" → "Run workflow".

### 4. Merge the PRs

Review and merge each PR. When merged, the site is automatically re-rendered and deployed with the new theme.

> [!NOTE]
> **Theme updates don't create new content versions.** The versioning system in notebook pubs (git tags like `v1.0`, `v1.1`) exists to track changes to scientific content—notebooks, figures, conclusions. Theme updates (CSS, fonts, layout) are orthogonal to the science and shouldn't create new versions. When a theme PR is merged, only the styling changes; the content versions remain unchanged.
