# notebook-pub-theme

A [Quarto extension](https://quarto.org/docs/extensions/) that provides shared styling and infrastructure for Arcadia Science notebook publications.

This extension provides all the styling for our notebook pub. It includes our assets (citation style, logo files, etc), all of the CSS (fonts, page layout, etc.), and interactive components (mini-title sticky header, author reveal, citation box, logo animation, etc.).

## How to use

This extension comes pre-packaged with our [notebook pub template](https://github.com/Arcadia-Science/notebook-pub-template). **Head there if you're trying to develop a notebook pub**, or continue reading if you plan to make changes to the theme shared across all our notebook pubs.

## Developer instructions

### Setup

Clone this repo and make sure you've [installed Quarto](https://quarto.org/docs/get-started/): `quarto --version`

### Testing theme changes

You must test theme changes in concert with the notebook-pub-template repo, which includes a demo notebook that exercises various styling features.

1. If you haven't already, clone the [notebook pub template](https://github.com/Arcadia-Science/notebook-pub-template).

2. Replace the existing extension with a symlink to the theme repo's contents:

   ```bash
   cd notebook-pub-template
   rm -rf _extensions/arcadia-science
   mkdir -p _extensions/arcadia-science
   ln -s ../notebook-pub-theme/_extensions/arcadia-science/arcadia-pub-theme _extensions/arcadia-science/arcadia-pub-theme
   ```

   (Modify the symlink source path to match your repo location)

3. Run the preview:

   ```bash
   make preview
   ```

4. Make changes to CSS, JS, or other extension files in `notebook-pub-theme`. The preview is live and will hot reload.

### Propagating changes

After merging changes to `main`, you'll need to update the repos that use this theme.

#### Updating notebook-pub-template

The template should always have the latest theme so new publications start with current styling:

```bash
cd notebook-pub-template
rm -rf _extensions/arcadia-science
quarto add Arcadia-Science/notebook-pub-theme --no-prompt
git add _extensions/arcadia-science
git commit -m "Update arcadia-pub-theme"
```

#### Updating existing publications

Each publication repo has its own copy of the theme extension. To pull in updates:

```bash
cd <publication-repo>
quarto update extension Arcadia-Science/notebook-pub-theme --no-prompt
git add _extensions/arcadia-science
git commit -m "Update arcadia-pub-theme"
```

Test locally with `make preview` before pushing.

### Future plans

We plan to automate theme updates using Dependabot or a similar tool. This would automatically open PRs in publication repos when the theme is updated, reducing manual effort and ensuring publications stay current with styling improvements and bug fixes.
