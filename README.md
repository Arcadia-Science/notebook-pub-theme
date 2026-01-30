# notebook-pub-theme

A Quarto extension that provides shared styling and infrastructure for Arcadia Science notebook publications.

## What this extension provides

- **CSS styling**: brand colors, typography, layout (navbar, footer, sidebar, article styling)
- **Interactive components**: Mini-title sticky header, author reveal with roles/ORCID, citation box modal, logo animation, version dropdown, etc.
- **Filters**: `abstract-section` filter that renders Summary sections with special styling
- **Shortcodes**: `iconify` shortcode for icons in navbar and content
- **Assets**: Arcadia citation style (CSL), logo files, etc.

## Installation

```bash
quarto add Arcadia-Science/notebook-pub-theme
```

Or clone/copy the extension into your project's `_extensions/` directory.

## Usage in a Publication

After installing, update your `_quarto.yml`:

```yaml
format: arcadia-science/arcadia-pub-theme-html

shortcodes:
  - _extensions/arcadia-science/arcadia-pub-theme/iconify.lua

filters:
  - _extensions/arcadia-science/arcadia-pub-theme/filters/abstract-section.lua
```

Your publication also needs an `assets/` directory with logo files:
- `logo_white.png`
- `logo_text.png`
- `logo_movie.mp4`

These can be copied from the extension's `assets/` directory.

## Developer Workflow

### Testing theme changes

To test changes to the theme extension, use the notebook-pub-template repo which includes a demo notebook that exercises various styling features:

1. Clone both repos:
   ```bash
   git clone https://github.com/Arcadia-Science/notebook-pub-template.git
   git clone https://github.com/Arcadia-Science/notebook-pub-theme.git
   ```

2. Symlink the extension into the template:
   ```bash
   cd notebook-pub-template
   rm -rf _extensions/arcadia-science
   mkdir -p _extensions/arcadia-science
   ln -s ../../../notebook-pub-theme/_extensions/arcadia-science/arcadia-pub-theme _extensions/arcadia-science/arcadia-pub-theme
   ```

3. Run the preview:
   ```bash
   make preview
   ```

4. Make changes to CSS, JS, or other extension files in `notebook-pub-theme` - the preview will hot-reload

### What lives where

| Content | Location | Rationale |
|---------|----------|-----------|
| CSS, JS, filters | Extension | Shared across all pubs, updated centrally |
| Logo assets | Both extension and pub | Pubs need copies for build; extension has canonical versions |
| Demo notebook | Template (`examples/`) | Used to test the theme during development |
| `_quarto.yml` config | Pub | Contains pub-specific settings |
| `authors.yml`, `_variables.yml` | Pub | Pub-specific metadata |

### Making Changes

1. **CSS changes**: Edit files in `css/`. The `main.css` imports all others.

2. **JavaScript changes**: Edit HTML files in `includes/`. These contain `<script>` tags with the JS.

3. **Filter changes**: Edit `filters/abstract-section.lua`.

4. **Adding new CSS variables**: Add to `css/colors.css` for colors or `css/main.css` for other variables.
