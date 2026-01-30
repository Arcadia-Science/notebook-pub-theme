# notebook-pub-theme

A Quarto extension that provides shared styling and infrastructure for Arcadia Science notebook publications.

## What This Extension Provides

- **CSS styling**: Arcadia brand colors, typography, layout (navbar, footer, sidebar, article styling)
- **Interactive components**: Mini-title sticky header, author reveal with roles/ORCID, citation box modal, logo animation, version dropdown
- **Filters**: `abstract-section` filter that renders Summary sections with special styling
- **Shortcodes**: `iconify` shortcode for icons in navbar and content
- **Assets**: Arcadia citation style (CSL), logo files

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

## Extension Structure

```
_extensions/arcadia-science/arcadia-pub-theme/
├── _extension.yml          # Extension configuration
├── css/                    # Stylesheets
│   ├── main.css           # Entry point (imports others, defines fonts)
│   ├── colors.css         # Arcadia color palette
│   ├── navbar.css         # Navigation styling
│   ├── mini-title.css     # Sticky header styling
│   ├── frontmatter.css    # Title block and author styling
│   ├── sidebar.css        # Table of contents styling
│   ├── article.css        # Content styling
│   ├── grid.css           # Layout grid
│   ├── citation-box.css   # Citation modal styling
│   └── footer.css         # Footer styling
├── includes/               # HTML components with JavaScript
│   ├── author-reveal.html # Dynamic author list from authors.yml
│   ├── mini-title.html    # Sticky header on scroll
│   ├── version-in-title.html
│   ├── citation-box.html  # Citation modal
│   └── logo-animation.html
├── assets/                 # Static assets
│   ├── arcadia.csl        # Citation style
│   ├── logo_white.png
│   ├── logo_text.png
│   └── logo_movie.mp4
├── filters/
│   └── abstract-section.lua
├── iconify.lua            # Iconify shortcode
└── iconify-icon.min.js    # Iconify runtime
```

## Developer Workflow

### Testing Theme Changes

To test changes to the extension:

1. Clone both this repo and [notebook-pub-template](https://github.com/Arcadia-Science/notebook-pub-template)

2. In notebook-pub-template, create a symlink to your local extension:
   ```bash
   rm -rf _extensions/arcadia-science/arcadia-pub-theme
   ln -s /path/to/notebook-pub-theme/_extensions/arcadia-science/arcadia-pub-theme \
         _extensions/arcadia-science/arcadia-pub-theme
   ```

3. Run preview in notebook-pub-template:
   ```bash
   quarto preview
   ```

4. Make changes to the extension files - the preview will hot-reload

The notebook-pub-template includes a demo notebook (`examples/demo.ipynb`) that exercises various styling features. Use this to verify your changes look correct.

### What Lives Where

| Content | Location | Rationale |
|---------|----------|-----------|
| CSS, JS, filters | Extension | Shared across all pubs, updated centrally |
| Logo assets | Both extension and pub | Pubs need copies for build; extension has canonical versions |
| Demo notebook | notebook-pub-template | Deleted when pub is finalized |
| `_quarto.yml` config | Pub | Contains pub-specific settings |
| `authors.yml`, `_variables.yml` | Pub | Pub-specific metadata |

### Making Changes

1. **CSS changes**: Edit files in `css/`. The `main.css` imports all others.

2. **JavaScript changes**: Edit HTML files in `includes/`. These contain `<script>` tags with the JS.

3. **Filter changes**: Edit `filters/abstract-section.lua`.

4. **Adding new CSS variables**: Add to `css/colors.css` for colors or `css/main.css` for other variables.

## Version History

See [CHANGELOG.md](CHANGELOG.md) for version history.
